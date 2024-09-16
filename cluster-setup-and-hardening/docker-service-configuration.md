# Docker Service Configuration

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Let’s look at how to configure the Docker Daemon app service.&#x20;
* In the previous lecture, we saw how to manage the Docker service using the systemctl service management utility.&#x20;
* We used the systemctl start command to start the Docker service, the status command to check the status of the service, and the stop command to stop the service.
* Note that this could be different on different operating systems depending on how you install Docker.
* In our case, we happen to be using Linux and we happen to have the systemctl based service configuration.
* Configuring Docker as a service helps run the Docker daemon in the background and automatically starts the service when the system boots up.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* You could also run Docker daemon directly as a foreground process using the dockerd command. This is usually done for troubleshooting and debugging purposes where if the Docker daemon is not starting up, you could run the Docker daemon this way and look at the logs and figure out what could be wrong. This prints out the daemon logs on screen.&#x20;
* To print additional details, use the debug flag along with the command. This will now add more detailed information in the output along with the debug logs.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* When the Docker daemon starts, it listens on an internal Unix socket at the path/var/run/Docker.sock.&#x20;
* This can be seen in the output of the logs. A Unix socket is an IPC or inter-process communication mechanism that is used for communication between different processes on the same host.&#x20;
* This means that the Docker daemon is only accessible within the same host and the Docker CLI is configured to interact with the Docker daemon on this socket.

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now, what if we need to establish connectivity to the Docker daemon from outside of this host? Say there is another host with the Docker CLI installed on it and we want to target the Docker host to run containers.
* Let's say the Docker host is a server somewhere hosted somewhere in your infrastructure and this host here is your laptop. You have Docker CLI installed on your laptop and from your laptop, you want to run Docker commands that can target this Docker host.
* As of now, with the current way of things are set up, the default way, the Docker daemon is not accessible outside of the Docker host because it's only listening on the Unix socket.
* We could make the Docker daemon listen on a TCP interface on the Docker host by adding the host option while running the Docker daemon command like this.

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* In this case, our host happens to have an interface at 192.168.1.10 and the standard port for Docker happens to be 2375. Now, the Docker daemon is accessible from outside of the Docker host using the IP 192.168.1.10 and the port 2375.
* The other host can now target the Docker daemon using the Docker commands by first setting an environment variable called DOCKER\_HOST to point to the IP address of the Docker host.
* Now, remember that there is a reason why this is disabled by default. You must be extremely careful before making your Docker API server available on an interface on your host, especially if your host is on the internet.
* This will allow anyone on the internet to run containers on this host because the default setting of Docker, if you make it available on a public interface, it's not encrypted and there's no authentication required.
* &#x20;That's the default setting. If you do this, then you have to be extremely careful and you should not do this on a public-facing host.

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* There have been numerous occurrences of hacks where people try to connect to your Docker host using the port 2375 and try and run containers for Bitcoin mining and things like that.
* If you are allowing external access, you must also take care of security. Now, by default, as I said, the Docker daemon serves unencrypted traffic.

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* To enable encryption, you must first create a pair of TLS certificates and add the TLS flag to true and passing the path to the certificate and key like this.&#x20;
* With TLS enabled, the standard port is now 2376, instead of 2375. 2375 is for unencrypted traffic, and 2376 is for encrypted traffic, so keep that in mind.
* Now, as of now, we have all the options specified while running the dockerd command as options in the command line. Now, these options could be moved to a configuration file at the location /etc/dockers/daemon.json.
* This is the default configuration file. The file is in a JSON format. This is not created by default, so you must create one if it does not already exist. Specify the options here in a key value format.
* Now, note that the host's property is an array that supports multiple listeners, so it has to be specified like this. Once this file is configured, we no longer have to pass in the parameters in the dockerd command.

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now, if you do happen to specify a flag, both in the daemon.json file and the command line, then the service throws an error that says that there's a conflict on an option, also known as a directive between the values specified in the configuration file and the daemon.json file.&#x20;
* In this case, on the command line, we set the value of debug to false, but in the daemon.json file, it’s set to true, so it's failing because those two are different.&#x20;
* This configuration file is also read and the values are interpreted when we run Docker as a service, using the systemctl start command. The options are passed through.
