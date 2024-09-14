# UFW Firewall Basics

<figure><img src="../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

* In this lecture, we will get introduced to UFW, which stands for Uncomplicated Firewall. Consider a set up where we need to restrict the network access on an Ubuntu server called app01. This server should only accept SSH connections from a single server with the IP address range 172.16.238.5. This is a jump server used by the system admins to SSH to app01.&#x20;
* This machine also runs a web server on port 80, which should also be accessible from the jump server and all the internal users connecting from the IP range of 172.16.100.0/28. Besides these requirements, all other ports on the server app01 should be closed, blocking the inbound access.

<figure><img src="../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

* To do this, we can make use of the internal packet filtering system available in the Linux kernel called net filter.&#x20;
* One of the common command line tools used to interface with the net filter is IP tables. Although IP tables is perhaps the most common tool used to configure the Linux firewall, it has a bit of a learning curve. For securing the setup that we just discussed, let us take a look at UFW.&#x20;
* It stands for Uncomplicated Firewall. UFW is a simple frontend interface for IP tables, which as the name suggests, provides a simple and easy to use interface to set up firewall rules.

<figure><img src="../.gitbook/assets/image (200).png" alt=""><figcaption></figcaption></figure>

* Before we begin, let's SSH to app01 and let us run the command netstat-AN to inspect the posts that are listening for connections. Sure enough, we'll find that the port 22 for SSH and HTTP port 80 are listening. There's another port 8080, which along with all the other ports should be blocked to inbound connections from everywhere else.

<figure><img src="../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

* To install the UFW tool on the app01 node, first update the packages list from the default repositories in Ubuntu using the apt update command. Next, use the apt-get install UFW command to install the UFW package. Once installed, enable and start the UFW service.

<figure><img src="../.gitbook/assets/image (202).png" alt=""><figcaption></figcaption></figure>

* Now, let's use UFW to manage firewall rules as per our requirements. Before we begin, let us check the status of the firewall by running the UFW status command.&#x20;
* It should show that the status is currently inactive, and this is the expected. We have not loaded any firewall rules and neither have we enabled the firewall yet. We will do that once we have added some rules. Let us first define some default rules.&#x20;
* On the app01 node, we have identified port 80 and 22 to be allowed from 2 sources. The outbound connections from app01 server has no restrictions.&#x20;
* Let us first add a default allow rule that will allow all outbound connections. To do this, we have run UFW default allow outgoing command as a root user. Let us also add a default deny for all inbound connections.&#x20;
* Don't worry this will not block any existing connections to port 80 and 22 yet as we have not yet enabled the firewall.
* This will be done once we have added the respective inbound routes from the tool source IP address ranges that we discussed.

<figure><img src="../.gitbook/assets/image (203).png" alt=""><figcaption></figcaption></figure>

* Now that we have added the default rules for both outgoing and incoming connections let us add allow rules. First, let us add an allow rule to permit SSH connections from the jump server with the IP address, 172.16.238.5. To do this, we have run the command UFW allow from 172.16.238.5 to any port 22 with protocol TCP.
* This command will allow inbound connection from the IP address 172.16.238.5 to the TCP port 22 on any reachable IP address on app01.
* Similarly, let us add and allow connection to port 80 from the same IP address. Next, let's has also add an allow rule to port 80 from the IP range, 172.16.100.0/28.
* Finally, we can also deny the port 8080 which we know is listening on app01. To do this, we have run the command UFW deny 8080. This will deny access to 8080 from everywhere.
* In our particular case, this is not really needed as we have blocked all incoming connections by default.

<figure><img src="../.gitbook/assets/image (204).png" alt=""><figcaption></figcaption></figure>

* Finally, to activate the firewall, run the command UFW enable. Particular care should be taken before we run this command, make sure that all applicable rules have been added to prevent any loss in connection.&#x20;
* Once enabled, we should be able to check the status of the rules by running the UFW status command.

<figure><img src="../.gitbook/assets/image (206).png" alt=""><figcaption></figcaption></figure>

* To delete a specific rule, for example, to delete the deny 8080 rule that we just added, we can use the command UFW delete deny 8080. Alternatively, we can also specify the line number associated with the rule that we want to delete.&#x20;
* For example, we can see from the UFW status command that the deny rule for port 8080 is created at rule number 4 and 5. Let's firstly delete the rule number five.
* To do this, we can run the command UFW delete 5. When we delete the rules this way the command will display the exact rule at that number that is going to be deleted. To proceed with the operation, enter Y when prompted.
* Once the rules five deleted, proceed with the rule number four the same way.
