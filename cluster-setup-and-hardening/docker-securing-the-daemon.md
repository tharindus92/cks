# Docker - Securing the Daemon

<figure><img src="../.gitbook/assets/image (10) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* First of all, what is the problem if someone gains access to their Docker daemon? Anyone with access to the Docker daemon can delete existing containers hosting applications, and thereby, impacting your users. If you happen to have stored your application data in the form of Docker volumes, then the person with access to the Docker daemon or the Docker API can delete volumes resulting in a data loss situation.
* Anyone with access to the Docker daemon can run containers to run their own applications. Say, for example, for Bitcoin mining purposes.
* Anyone with access to the Docker daemon can gain root access to the host system itself by running a privileged container. We’ll look at what a privileged container is in a bit.
* Once they gain root access to the host system, they can target other systems in the environment and the whole network infrastructure itself.
* Earlier, when we talked about configuring the Docker daemon, we discussed about the different access methods. We said that by default, Docker exposes the Docker daemon service within the host only on a UNIX socket, and this prevents anyone outside of the host from accessing the Docker daemon.
* That’s why this is set to that option by default.
* Note that we’re talking about the Docker daemon service itself and not the applications deployed by Docker. The applications deployed can still be accessed as long as we publish ports on those applications on the hosts. Here, we’re referring to the Docker Engine, the Docker API server itself.
* As of now, only someone who is logged in to this host has access to the Docker service. The first level of security that we must think about is securing the Docker host itself from unwanted access.
* As you would secure any server, you must secure the Docker host with the usual security best practices of securing a server such as disabling root users, controlling who has access to the server, disabling password-based authentication, strictly enforcing SSH key-based authentication, disabling any unused ports, et cetera.
* We also discussed about configuring external access to the Docker daemon. At times, you may really have to allow access to the Docker daemon from another management host, or say, a user’s laptop, or for integration purposes with other tools.

<figure><img src="../.gitbook/assets/image (11) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now this must be done only if it’s absolutely necessary. For this, we add the "hosts" option to the daemon. json file, and that now enables access to the Docker daemon from outside of the Docker host itself through an interface on our host.Now this must be done only if it’s absolutely necessary. For this, we add the "hosts" option to the daemon. json file, and that now enables access to the Docker daemon from outside of the Docker host itself through an interface on our host.
* If you have to do this, make sure you expose the Docker daemon on interfaces that are private and only accessible from within your organization. So if your host has a public-facing interface, then make sure the daemon is not exposed on those interfaces.
* By allowing external access, we must also secure communication with TLS certificates. For this, we configure a Certificate Authority, a CA, and create certificates for the server. We will call it "server. pem," and "serverkey.pem". We place these on the Docker host and configure the daemon.json file to read them along with setting the "tls" option to true. The port is now 2376 instead of 2375.

<figure><img src="../.gitbook/assets/image (12) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* With this, anyone outside of the Docker host can access the Docker API and target it with the Docker commands by simply setting the Docker host environment variable to point to this host.
* With encryption enabled, you must set the TLS flag to initiate secure connection. For this, set the Docker TLS environment variable to true.
* With encryption enabled, you must set the TLS flag to initiate secure connection. For this, set the Docker TLS environment variable to true.
* Now note that any of these environment variables being set can also be passed in as a command-line parameter like this, but since we have already set the environment variable, we don’t need it so we will get rid of it.

<figure><img src="../.gitbook/assets/image (13) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* With this setup, the Docker CLI on the Docker client and the Docker server starts communicating in an encrypted manner. However, there is still no authentication, meaning anyone who knows that the Docker daemon is being exposed on port 2376 on this host can still access our Docker daemon by simply setting the Docker TLS environment variable to true and pointing the Docker host environment variable to our host, and they can start deploying containers on our host or delete existing containers.
* The next step is to enable certificate-based authentication.
* &#x20;For this, we copy the "cacert" to the Docker daemon server-side and configure "tlscacert" parameter in the daemon. json file along with the "tlsverify" flag to true.

<figure><img src="../.gitbook/assets/image (14) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* The "tlsverify" flag is what enables authentication and the "cacert" will be used to verify certificates of clients when they send them. Our clients now need certificates signed by the CA. That’s the only way now they can access the Docker daemon.
* We generate certificates for our clients called "client. pem" and "clientkey. pem", and share that securely with the client-server that wants to connect to the host, along with the "cacert", the certificate of the CA.

<figure><img src="../.gitbook/assets/image (15) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* On the client-side, we set the Docker TLS verify flag to true instead of the TLS flag we set earlier, and then we either pass in the client certificate through the command-line parameter, or we drop the certificate into the .docker directory under the user’s home directory.
* From there, the Docker command will automatically pick up the certificates.

<figure><img src="../.gitbook/assets/image (16) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now, only clients with signed certificates from CA server can access the Docker daemon.
* Remember, the TLS option alone does not authenticate, it only enables encryption and anyone can still access the Docker daemon with the TLS flag enabled.
* The TLS verify option is what enables authentication.&#x20;
* To summarize, without authentication and just to enable encryption, we configure the TLS flag and the TLS certificate and key flag on the server. On the client-side, we just set the TLS flag to true. This can either be set on the command line as the "--tls" option, or an environment variable called Docker\_TLS.

<figure><img src="../.gitbook/assets/image (17) (1) (1).png" alt=""><figcaption></figcaption></figure>

* With authentication on the server-side, we set the TLS verify flag along with the server certificate and key, and the CA certificate as well. On the client-side, we set the TLS verify flag to true along with the TLS certificate, TLS key, and TLS CA certificate passed in either as command-line parameter or environment variable, or placed in the .docker directory under the user’s home directory.

<figure><img src="../.gitbook/assets/image (18) (1) (1).png" alt=""><figcaption></figcaption></figure>
