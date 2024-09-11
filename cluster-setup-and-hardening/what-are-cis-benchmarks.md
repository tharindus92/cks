# What are CIS Benchmarks



* Hello, and welcome to this lecture. In this lecture, we'll look at what CIS benchmarks are. Let's start by understanding what a security benchmark is first.
* &#x20;If you have worked as a systems administrator and have performed security audits on systems, then you probably know this already. Here, I have a Linux system, Ubuntu 18.04, to be specific, and I just deployed it as a fresh operating system on a server in my data center. Before we host our production-grade applications on the system, it is important that we secure it.&#x20;
* With respect to security, there are so many ways the system is vulnerable to attacks.&#x20;
* For example, someone who sneaks into the data center can attach a USB drive to the server, and infect the server with virus or malware. USB ports or peripheral slots on the server that are not planned to be used must be disabled to prevent such an attack.
* From an access perspective, what user accounts are to be configured, and which users have access to the system, and users log in as root. What if an admin logs in as root and performs changes that impact the server, you wouldn't know which user did that.
* Instead, security best practices recommend that the root user must be disabled and only allow admins to log in using their own account and use Sudo to elevate their permission levels if required. That brings us to the next requirement, which is configuring Sudo. A server must have Sudo configured and only necessary users configured to have Sudo privileges.
* There are network-related best practices, such as to configure firewalls or IP tables to open up only required ports from and to specific IP addresses.&#x20;
* From a services perspective, we must make sure that only necessary services are enabled, such as NTP service for time synchronization, and all other services that are not required on the server are to be disabled.
* From a file system standpoint, we must ensure that the right permissions are set for files, and unused file systems are disabled.&#x20;
* We must make sure to configure auditing and logging on the system to make sure all changes are logged for auditing purposes.
* These are just a few among the many, many best practices to be followed. A new vulnerability emerge every now and then, and we must make sure that we upgrade patch and configure our servers to prevent these new types of attacks.
* Where are these best practices available? How can we see where our server stands with respect to implementing these best practices?

<figure><img src="../.gitbook/assets/image (29) (1).png" alt=""><figcaption></figcaption></figure>

*   There are many tools available to assess our servers. One of the most commonly used tools is the CIS benchmarking tool.

    <figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
* CIS stands for the Center for Internet Security. It is a community-driven nonprofit organization whose mission is to make the connected world a safer place by developing, validating, and promoting timely best practice solutions that help people, businesses, and governments protect themselves against pervasive cyber threats.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* &#x20;The CIS website provides cybersecurity benchmarks for over 25 different vendors that include operating systems like Linux, Windows, or Mac OS, public cloud platforms like Google, Azure, AWS, and others.&#x20;
* Mobile platforms like iOS and Android, network devices like Checkpoint, Cisco devices, Juniper, Palo Alto Networks, desktop software's like web browsers, MS Office, Zoom, and server software like different web servers, like Tomcat and Nginx, and other popular databases, as well as those that fall into the virtualization category, such as VMware, Docker, and Kubernetes. You may register at the CIS website and download CIS benchmarks for the necessary components of your choice.
* Now, within these benchmarks are pages of instructions of various security recommendations, with each recommendation detailing why it is a threat, how to check if the threat exists on your system, which includes the commands that you must run as well, and how to remediate the same. Again, that includes the commands to remediate that problem as well.

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now, not only does CIS publish best practices, they also provide necessary tools that can help run assessments and remediate them as well. The CIS-CAT tools help in the automated assessment of the server. It compares the current configuration on the server against the recommended security best practices in the benchmark document and reports issues if any are found.

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Once run, it generates a report in an HTML format summarizing the list of recommendations outlined in the best practices and a status indicating if these have been implemented on the system.
* &#x20;In this screenshot from the results page of a test run, shows the tests that passed or failed for each category of tests and associated score as well. A detailed breakdown of each test and corresponding results can also be seen when you click each of these groups.
