# AppArmor

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

* &#x20;In this lecture, we will learn about AppArmor and see how it can be used to further restrict an application’s capabilities to reduce the attack surface of a threat. In the previous lecture, we learned about seccomp profiles and saw how they can be used in Kubernetes to restrict the syscalls for a pod.&#x20;
* Although we can make use of seccomp to effectively restrict which syscalls a container can or cannot use, we cannot use it to restrict a program’s access to specific objects, such as a file or a directory. Let us use the same example that we used in the seccomp lecture.
* By restricting the mkdir syscall using a custom profile, we can effectively prevent the container from creating directories inside it.&#x20;
* However, how do we limit the container to, say, prevent writing to a file system or a specific directory? To implement such fine-grain control over the processes running inside a container, we have to make use of AppArmor.&#x20;
* AppArmor is a Linux security module which is used to confine a program to a limited set of resources.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

* AppArmor is installed by default in most Linux distributions. To check if it is installed and running, use the systemctl status AppArmor command like this.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

* To make use of AppArmor, the AppArmor kernel module must first be loaded on all the nodes where the container would run.&#x20;
* Now, this can be verified by checking the enabled file under the /sys/module/apparmor/paramiters directory. A value of Y or yes implies that AppArmor kernel module is loaded. Just like seccomp, AppArmor is applied to an application via a profile.&#x20;
* This profile, which we’ll take a look in a minute, must be loaded into the kernel. This can be verified by checking the /sys/kernel/security/AppArmor/profiles file like this. Now, in this case, we can see that several profiles have already been loaded on this host.&#x20;
* AppArmor profiles are simple text files that define what resources can be used by an application. These include Linux capabilities, network resources, file resources, et cetera.

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

* For example, a profile that restricts all file rights within a file system can be defined like this. This simple profile called apparmor-deny-write contains two rules.
* &#x20;The first rule here is file, which is a shorthand for allow file system. This rule allows complete access to the entire file system. Next, we have the deny rule, which prevents write access to all files under the root file system, including the sub-directories.&#x20;
* This basically implies denying all writes to the entire file system. Let’s use another example. If you only want to restrict writes to the files directly under the /proc file system, we can use a deny rule like this.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

* Here’s another example. To restrict the remounting of the root file system as read-only, we can make use of a deny rule like this.&#x20;
* Now, don’t worry if the profile looks confusing at the moment. We will learn how to create profiles using AppArmor tools in the next lecture.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

* The AppArmor profiles that are loaded and the status can be checked using the aa-status tool. Here, we can see that AppArmor module and 12 AppArmor profiles are loaded into the kernel.&#x20;
* Profiles can be loaded in three different modes, enforce, complain, and unconfined. With enforce mode, AppArmor will monitor and enforce the rules that we just saw on any application that fits the profile. When a profile is in a complain mode, AppArmor will allow the application to perform tasks without any restriction, but it will then log them as events.
* &#x20;Finally, the unconfined mode, as the name suggests, allows application to perform any tasks. In this case, AppArmor does not log it as events.
