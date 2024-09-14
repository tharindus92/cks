# Disable Open Ports

<figure><img src="../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>

* In this lecture, let us see how to inspect a system for open ports. Once the ports have been identified, we will later look at ways to disable them on a Linux host.&#x20;
* Earlier in this lecture we saw how process startup can be managed using systemd services. We learned that just like unwanted packages it's a good idea to disable and remove unwanted services on the host. Several processes, when they are started, also bind to a port.&#x20;
* A port is an addressable location in the operating system that allows for the segregation of network traffic intended for different applications.&#x20;
* For example, the TCP port 22 is only used when an SSH server process is standard on the machine.

<figure><img src="../.gitbook/assets/image (195).png" alt=""><figcaption></figcaption></figure>

* To check if a port is used and listening for connection requests, we can make use of the netstat command.
* In this example, besides port 22 for SSH, we can also see open ports, such as 2379 for the ACD instance, 6443 for the Kubernetes API server, et cetera.
* This is expected, as the server in this example is being used as a Kubernetes control plane node. Now that we have a list of ports that are running on the system, how do we know what each of them are uses for?
* Well, one simple way is to check the etc services file, if you're using an Ubuntu base distribution. Let's choose information about several services on the system, including the service name, the protocol, and the port numbers.

<figure><img src="../.gitbook/assets/image (196).png" alt=""><figcaption></figcaption></figure>

* For example, if you want to look at what port 53 is used for, you can check the services file like this. This shows that the port 53, both TCP and UDP is reserved for the domain name server or the DNS.

<figure><img src="../.gitbook/assets/image (197).png" alt=""><figcaption></figcaption></figure>

* How do we know which port should be open when installing a new software? Well, the simple answer is to make use of the reference documentation for installing that particular software.
* &#x20;For example, if the cluster is being installed using the kubeadm tool, here are all the ports that need to be open for the cluster to do work properly.&#x20;
* This may vary if you decide to use a different Kubernetes provider such as Rancher or OpenShift, for example.
* Make sure that you use the appropriate documentation before installing the software. Once we know which ports are needed, disable and block the ones that are not needed in the system. We will learn how to do that in the upcoming lecture.
