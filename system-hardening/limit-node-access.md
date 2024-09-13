# Limit Node Access

<figure><img src="../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

* Let's start with the actual nodes of the Kubernetes cluster, and see how we can minimize the attack surface by limiting the access to the hosts. In the early section, we learned about the different ways a user can access a Kubernetes cluster, such as using A-bace and R-bace.
* &#x20;What about accessing the nodes themselves? As a standard practice, we should limit exposure of the control plane and the nodes to the internet. Please know that if you're running a managed Kubernetes service on the cloud, it is likely that in some cases you won't be able to access the control plane nodes anyway.&#x20;
* However, for self-hosted clusters, or in some managed Kubernetes providers, provisioning the nodes in a private network is one way to achieve this. With public access disabled, we can then securely access the cluster via a VPN solution.

<figure><img src="../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

* If implementing a VPN solution to access the private network isn't possible, another alternative is to enable authorized networks within the infrastructure firewall.&#x20;
* This option allows restricted access to the nodes based on the source IP address range that we can define on the infrastructure firewall.
* &#x20;For example, we can allow connections from the IP range, say 44.44.44.0/24, and block connections from another range, such as 33.33.33.0/24. We can also block connectivity directly on the hosts themselves and we will look how to do that in the upcoming lectures.

<figure><img src="../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

* Of course, the tech can be internal as well, by entities that already have network-level access to the Kubernetes nodes. Ask yourself this, who would need SSH access to the nodes of your cluster? System administrators, for one, would need access as they manage the nodes, making sure that it is in a working state, upgraded, and passed from time to time, and, of course, to troubleshoot and fix issues on the node.&#x20;
* Do the developers need access to the nodes? The answer to this should be no. However, it may depend on the environment as well. For example, if it's a production system, then the developer should not be able to access the nodes. This may be an exception in case of a development cluster.&#x20;
* Now, of course, as we just saw, the end-users of the application are external users, and they should not be able to access the nodes at all.

<figure><img src="../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

* Let's now take a look at how we can manage these user accounts in Linux. There are four types of accounts.&#x20;
* The user account refers to individuals who need access to the Linux system. Examples of a user account are those belonging to system administrators and developers. A superuser account is the root account, which has the UID of zero.&#x20;
* This superuser has unrestricted access and control over the system, including other users. System accounts are typically created during the OS installation.&#x20;
* These are for the use by software and services that will not run as the superuser. Service accounts are similar to the system accounts and are created when services are installed in Linux. For example, an Nginx service that makes use of a service account called "Nginx."

<figure><img src="../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

* To see details about users in Linux, we can make use of some very simple commands.&#x20;
* The ID command gives information about the user, specifically the UID, GID, and what other groups the user is part of.&#x20;
* The "who" command is used to see the list of users currently logged into the system.&#x20;
* Finally, the last command lists the last time users were logged into the system.

<figure><img src="../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

* Let us now look at access control files. Most of the access control files in Linux are stored under the /etc or the /etc directory.&#x20;
* The first one of interest is the password file. This file contains basic information about the users in the system, including the username, the UID, GID, home directory, and the default shell. It is important to note that this file itself does not save any passwords.&#x20;
* The passwords are stored in the next file that we are going to see, which is the /etc/shadow file. The contents of this file is hashed.&#x20;
* The /etc/group file stores information about all the user groups on the system, such as the group name, the GID, and the members belonging to the group.

<figure><img src="../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>

* Based on the information in the access control files, and using commands such as "ID," "who," "last," et cetera, we can investigate and analyze which accounts have access in the operating system.
* Adhering to the principle of least privilege, we should then remove or disable any account that has no use for in the system.
* We can disable the user account by updating the default shell for the user to a nologin shell like this.
* When we set the nologin shell for a user, just as the name suggests, the user will not be able to log into the host.
* Alternatively, we can also delete the account altogether using the "userdel" command.

<figure><img src="../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

* Similarly, we should also remove users from a group to which they shouldn't belong. For example, we can see that Michael is part of the admin group. To remove him from this group on an Ubuntu system, use the "deluser" command like this.
* Note that the topics discussed here are for local system accounts only, and do not take into account those that are in directory services such as the Active Directory or LDAP.
