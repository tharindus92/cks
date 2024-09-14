# Minimize external access to the network

<figure><img src="../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>

* In this lecture, we will get introduced to tools that can be used to restrict network access to our servers. In the previous lecture, we saw that services and processes can bind to a port listening for incoming connections.&#x20;
* For example, the SSH server, if running, will bind to the port 22 on the server. We then saw how to check for port-to-servers mappings by inspecting the /etc/services file.&#x20;
* To check if the port is listening for incoming connections on the server, we can make use of the netstat command. Since port 22 is listening on the IP address 0.0.0.0 without any additional configuration, any device within the network can establish connection to the server on this port.&#x20;

<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Here, we have just looked at the SSH port. Using the netstat command, we can see that there are several other ports that are open on the system and they can accept connection from any other device on the network.
* Again, going by the principle of least privilege, this not a desirable configuration.

<figure><img src="../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>

* In a real-world environment that has many different clients and many different servers all connected to a large network with many different switches and routers, it is important that we implement network security to allow or restrict access to the various services and ports such as which servers allow SSH access or which servers can access what other services on which ports, et cetera.&#x20;
* We can apply such security either network-wide using external firewalls or appliances such as the Cisco ASA, Juniper Next-Gen Firewall, Barracuda Next-Gen Firewall, Fortinet, et cetera.
* Using these appliances, you can define rules that will control any traffic flowing in and out of the network.

<figure><img src="../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption></figcaption></figure>

* An alternate option is to apply these rules at an individual server level using IP tables, FirewallD, NuFW in Linux-based systems, and firewalls in Windows servers.
