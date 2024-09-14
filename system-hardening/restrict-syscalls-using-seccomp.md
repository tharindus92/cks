# Restrict syscalls using seccomp

<figure><img src="../.gitbook/assets/image (207).png" alt=""><figcaption></figcaption></figure>

* In the previous lecture, we saw how applications running in the user space make use of SYSCALLS to request for resources. In this lecture, we'll learn how to allow programs to only make use of SYSCALLS that they absolutely need.&#x20;
* We saw that a simple command such as touch can invoke multiple SYSCALLS to the Kernel. Currently, there are about 435 SYSCALLS in Linux and all of them can be used by applications running in the user space to carry out hardware and networking tasks.
* However, in reality, no application would ever need to make these many SYSCALLS, and having complete access for programs to all of these SYSCALLS can increase the attack surface.

<figure><img src="../.gitbook/assets/image (208).png" alt=""><figcaption></figcaption></figure>

* In 2016, a vulnerability called Dirty Cow demonstrated how a program was able to make use of the SYSCALL called ptrace to write to a read-only file, then gain access to root and then even break out of the container. We have a separate video later in this course that demonstrates how this exploit can be used in certain Linux Kernels.

<figure><img src="../.gitbook/assets/image (209).png" alt=""><figcaption></figcaption></figure>

* How do we restrict applications whether they are directly running on the host, or in a container to only use SYSCALLS they actually need to function.
* By default, the Linux Kernel will allow any SYSCALL to be invoked by programs running inside the user-space which can increase the attack surface.
* This is where we can make use of SECCOMP. SECCOMP stands for secure computing and it is a Linux Kernel level feature that can be used to sandbox applications to only use the SYSCALLS they need.

<figure><img src="../.gitbook/assets/image (210).png" alt=""><figcaption></figcaption></figure>

* SECCOMP was first introduced in 2005 and has been part of the Linux Kernel since the version 2.6.12. To check if the kernel in the host supports SECCOMP, check the boot configuration by looking for the keyword SECCOMP in the boot config file for the kernel in use like this.&#x20;
* If you see an output like this with CONFIG\_SECCOMP=yes, that means that SECCOMP is supported by the kernel. To demonstrate how application security can be fine-tuned with SECCOMP, let us first run a simple container using the popular whalesay image from docker.

<figure><img src="../.gitbook/assets/image (211).png" alt=""><figcaption></figcaption></figure>

* This container will print the ASCII image of the docker wheel along with any argument that is passed in, which in this case is hello.&#x20;
* Now that we have established that the container is working as expected, let us run another container using the same image but this time, let us exact into the container.&#x20;
* Once in, let us attempt to change the system time. As expected, this did not work but why is that? The process being run inside this container is the shell/bin/sh.
* If we inspect the process ID, we can see that it has a PID of 1. Let us inspect the status of this process and specifically look for the SECCOMP field. To do this, let us look under the /proc file system under the status file for the PID of 1. You should see a result like this. Here we can see that the field SECCOMP has a value of two.

<figure><img src="../.gitbook/assets/image (212).png" alt=""><figcaption></figcaption></figure>

&#x20;

* A value of two means that SECCOMP is implemented on this container. SECCOMP can operate with three modes. Mode 0 implies that SECCOMP is disabled.&#x20;
* With mode 1, SECCOMP is applied in a strict mode that will block almost all SYSCALLS except for four which is read, write, exit, and cigarette on SYSCALLS.&#x20;
* The mode 2 which we saw was implemented on our container, selectively filters SYSCALLS.
* &#x20;Wait a minute, we did not tell docker to make use of SECCOMP when we ran the docker run command. So how was SECCOMP filter applied on this container?
* More importantly, what restrictions is imposed on this container right now? To answer the first question, docker has a built-in SECCOMP filter that it uses by default whenever we create a container provided that the host kernel has SECCOMP enabled which is true in this case.

<figure><img src="../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

* This filter which is described as a JSON document restricts about 60 of the 300 plus SYSCALLS in Linux including the ptrace SYSCALLS which as we learned earlier was at the heart of the Dirty Cow exploit.
* Please note that the JSON document that you see here has been truncated to fit the screen. Let us now take a closer look at this JSON document.

<figure><img src="../.gitbook/assets/image (214).png" alt=""><figcaption></figcaption></figure>

* A SECCOMP profile primarily consists of three elements. First is the architecture such as x86 64-bit, and 32-bit which define which systems the profile can be used for. Then we have the SYSCALLS array in which we can define an array of SYSCALL names and an associated action whether to allow or deny them. In this particular case, we have three SYSCALLS with the action of SCMP\_ACT\_ALLOW.&#x20;
* This means that when implemented, this profile will permit these three SYSCALLS to be used by the application. Finally, we have the default action which defines the default behavior when dealing with SYSCALLS that have not been declared inside the SYSCALLS array. In this case, the default action is set to SCMP\_ACT\_ERRNO, which will reject all other SYSCALLS by default.&#x20;
* This profile is an example of a whitelist profile as it only allows SYSCALLS declared within it and blocks everything else by default. Such a profile is highly secure but it is very restrictive.
* If a SYSCALL needed by the application is not added to the white list, it can cause the application to fail. Before using a whitelist type SECCOMP profile, you would have to identify every single SYSCALL that will be made by the application.
* There is another type of SECCOMP profile that we can use which is a blacklist type profile.
* A blacklist does the opposite of a whitelist. It allows every SYSCALL by default and rejects those that we have specifically defined inside the SYSCALLS array. Here, we have the default action of SCMP\_ACT\_ALLOW.
* The action inside the SYSCALLS array is set to SCMP\_ACT\_ERRNO. Blacklist profiles are way more open than the white list, and they are easier to create as compared to the white list.
* However, they are also more susceptible to attacks as they allow all SYSCALLS by default. If a potentially dangerous SYSCALL is not added to the blacklist, this can lead to security incidents.

<figure><img src="../.gitbook/assets/image (215).png" alt=""><figcaption></figcaption></figure>

* The default docker SECCOMP profile that we saw earlier, blocks around 60 of the 300 plus SYSCALLS on the X86 architecture.&#x20;
* These include SYSCALLS such as reboot, setting and manipulating the system clock, mounting and unmounting file system, creating and deleting kernel modules et cetera.
* Now going back to our earlier test, we were not able to change the system time on the whalesay container, and this is because as we just learned the SYSCALLS for clock adjust time, and clock set time are disabled by docker by default.
* For the complete list of all the block SYSCALLS by the default docker profile, please check the reference article following this lecture.

<figure><img src="../.gitbook/assets/image (216).png" alt=""><figcaption></figcaption></figure>

* While the default SECCOMP profile is great at restricting many SYSCALLS that has absolutely no use in a container, it is still quite open. Luckily, we can make use of custom filters with docker to further sandbox the containers.
* &#x20;For example, we can modify the default SECCOMP filter and remove the MKDIR SYSCALL and save it to a file called custom.JSON. Next, we can create another container by making use of the --Security option flag to make use of the custom SECCOMP profile.&#x20;
* Now the container cannot create directories inside it as our custom profile restricts it.

<figure><img src="../.gitbook/assets/image (217).png" alt=""><figcaption></figcaption></figure>

* &#x20;We can also disable making use of SECCOMP completely by using the SECCOMP as equal to an unconfined flag like this.&#x20;
* This will allow us to make use of all the SYSCALLS from within the container. Needless to say that this setting should never be used and you should always resort to using custom profiles to restrict or allow specific SYSCALLS whenever necessary.&#x20;
* One thing to note before we move on to the next lecture is that if you attempt to change the system time, even if the container was started without a second profile, this will still not work, and this is because there are additional security gates that are built into docker which will prevent you from running several SYSCALLS even with SECCOMP disabled.
