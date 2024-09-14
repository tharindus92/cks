# Linux Capabilities

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

* In this lecture, we will learn how to add or drop Linux capabilities on Kubernetes pods. Earlier in the lecture for seccomp, we saw that even when we run a container with seccomp set to unconfined, we were still not able to change the date within a container.&#x20;
* The same is applicable with a Kubernetes pod. When we run a pod in Kubernetes, it does not use seccomp by default. In spite of the container being run as the root user with the UID of zero, we still cannot change the date. Why is that so?

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

* To understand this, let us visit the fundamentals of how processes run in Linux. In older Linux servers with kernel older than 2.2, processes were segregated to two types, privileged processes and unprivileged processes.
* &#x20;The privileged processes were the ones that were run by the root user with the UID of zero and it could pretty much do anything on the server, bypassing the kernel permission checks. Unprivileged processes were those that were run by any other user whose UID is nonzero.&#x20;
* These processes had a lot of restrictions imposed by the kernel. From Linux kernel 2.2 onwards, the super users' privileges were divided into distinct units known as capabilities.
* &#x20;Now, instead of giving all the processes created by the root user privileges to do anything on the server, we can assign processes only a subset of capabilities.&#x20;
* For example, the CHOWN capability allows making user and group ownership changes to a file. The net\_admin capability allows various network-related operations such as network interface configurations, modifying routing tables, bind processes to an address, et cetera.&#x20;
* The sys\_boot capability allows the processes to reboot the system. The sys\_time capability allows setting and adjusting the system clock.

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

* There are dozens of these capabilities, each group based on their functionality. For the complete list, please refer to the reference article following this lecture.

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

* To check which capabilities a command needs, we can make use of the getcaps command. For example, here, we can see that the ping command makes use of the net\_raw capability.&#x20;
* Similarly, to check the capabilities of a process, we can make use of the getpcaps command.&#x20;
* For example, to get the capabilities of the SSH process, first, find the PID of this process, and then use the getpcaps command like this.

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

* Now, that we have understood the basics of Linux capabilities, let's go back to our ubuntu-sleeper pod.&#x20;
* When we attempted to change the date from within a pod, it was not permitted.&#x20;
* This is because a container, even if run as a root user, is started with a limited set of capabilities. In fact, when using Docker as the runtime, containers are only started with 14 capabilities by default and without the sys\_time capability that we saw earlier is required to set and adjust the system clock.

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

* Capabilities can however be added or removed for a container by specifying the capabilities field in the security context section of the container manifests like this.
* Once the pod is created, we can now change the date as expected.

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

* Similarly, to drop a capability, use the drop field followed by an array of capabilities to be removed, like this. Here, we have removed the CHOWN capability from the container. Once this pod is created, we can no longer use the CHOWN command to change the ownership of a file inside the container.
