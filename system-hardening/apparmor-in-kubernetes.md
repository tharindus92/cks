# AppArmor in Kubernetes

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

* In this lecture, we will learn how to use AppArmor profiles in Kubernetes. Just like in the case of traditional apps, AppArmor helps form a more secure deployment by restricting what container can or cannot do.&#x20;
* The support for AppArmor was added in Kubernetes version 1.4, however, it is important to note that as of this recording and version 1.20, it is still in beta. The prerequisites to make use of AppArmor and Kubernetes pod are still the same.&#x20;
* The AppArmor Kernel Module must be enabled on all the nods where the pods are expected to run. Similarly, the AppArmor Profile must also be loaded on all the notes.&#x20;
* The continue runtime used by Kubernetes should also support AppArmor, which is true for most popular runtimes, such as Docker, Cryo, Containerd, etcetera.

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

* Let us make use of a simple example and open to sleeper pod, which runs an ubuntu container that echoes a message and sleep for one hour.&#x20;
* The command running on this container does not need write access to the file system, let's make use of an AppArmor deny-write profile to secure this pod.&#x20;
* Before we proceed, let's make sure that this profile is loaded on all the nods where this pod can be scheduled.

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

* This can be done by running the aa-status command and making sure that the profile is loaded on the working nod. Now, run the same command on all the nods of the cluster and verify that the profile is loaded there as well.

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

* AppArmor profiles are applied per container, and since it is still in beta to use the AppArmor Profile on a container we have to add as an annotation to the pod submitted data. In this annotation, with the key container.apparmor. security.beta.Kubernetes.io, specify the container name.
* For the value, specify local host followed by the profile name.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

* For this example, the name of the container is ubuntu-sleeper, and the profile name is apparmor-deny-write.

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

* Next create the pod. Once it is in a running status, inspect the logs to see the message printed by the container.&#x20;
* Let's also test this profile by getting a shell into the container and attempt to create a file. As expected, this would fail as our profile denies writes to files and directories under the root file system.
