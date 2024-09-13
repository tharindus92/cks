# SSH Hardening

<figure><img src="../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>

* In this lecture, we will look at how to improve the security of our notes by securing the SSH service. SSH is used for logging into our remote Linux server and for executing commands on it.&#x20;
* The basic syntax of doing this is to run the SSH command, followed by the IP address or hostname of the server you want to connect to
* . You can also pass on the user you want to connect as with the SSH command by making use of the \[@] symbol before the hostname, or by making use of the -l, like this. The remote server, however, should have an SSH service running and port 22 accessible from the client for the connection to work.
* Another requirement is a valid username and password, which should be created on the remote system, or something known as an SSH key that you can use to log into the remote machine without a password.
* We will take a look at SSH key soon, but before that, let's see an example. If you want to connect to a Linux source from your laptop, you can issue the SSH command this way, SSH Node01, where Node01 is the host name of one of the notes of the cluster.
* Since the username has not been specified, it will try to log in as Mark, which is the current logged in user on the localhost.
* If the connection to the remote server is successful, it'll ask you to enter the password of the user Mark on the remote system. If you provide a valid password, you should then be taken to the shell on Node01.
* In this example, your laptop from which you're trying to connect as the client, and Node01, where the SSH service is running, is the server.

<figure><img src="../.gitbook/assets/image (189).png" alt=""><figcaption></figcaption></figure>

* What we have seen here is a very simple way to access a remote server using the SSH client by making use of the username and password as the means of authentication. A more secure way to do this is to make use of cryptographic key pair that makes use of private and public keys to authenticate to the system.
* To do this, we need to first generate a key pair on the client. A private key. Consider this as the key only you as the client will have and is not shared with anyone else. The public key. As the name suggests, it's public, and hence it can be shared with others, including our remote server. When the public key is installed on the remote server, you can unlock it by connecting to it with the client that already has the private key.



<figure><img src="../.gitbook/assets/image (190).png" alt=""><figcaption></figcaption></figure>

* First, create the key pair using the SSH keygen command on the client, which, for example, can be a laptop. When you're on this command, it will ask you to enter a passphrase. This is an optional step, but improves the security of the key should it become compromised. The only downside of using a passphrase is then having to type it in each time you use the key pair.
* From the output of the command, we can see that the public key is stored under the user's home directory inside a directory called .ssh, and the name has got a .pub extension.
* Here, the public key is called id\_rsa.pub. The private key is also stored in the same hidden directory, and it's called id\_rsa.

<figure><img src="../.gitbook/assets/image (191).png" alt=""><figcaption></figcaption></figure>

* The next step is to copy the public key to the remote server. To do this, you'll have to resort to a password-based authentication at least once. An easy way to do this is to make use of the command called SSH Copy ID, which should be installed by default. Use the command as shown here, specifying the user, which in our case is Mark, and the remote server name or IP.
* Once you enter the command, you will be asked to enter the password for your user on the remote server, and once this is done, you should be able to access the remote server without entering a password.

<figure><img src="../.gitbook/assets/image (192).png" alt=""><figcaption></figcaption></figure>

* The public key is installed in a specific file called authorized keys, which should be inside a hidden directory called .ssh in the User Home Directory.

<figure><img src="../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

* Now that we have learned about user accounts and passwordless SSH, we can update the SSH configuration to improve the security of the system. First, let us disable the SSH for the root account. This ensures that no one is able to log in remotely using root account, and only make use of their own account to connect to the system.
* Adhering to the principle of least privilege, we should never make use of the root account as a daily driver.
* Instead, use regular system accounts and use privilege escalation systems like pseudo whenever elevated access is required. To disable root logins over SSH, we have to update the SSH configuration file, which is located under /etc/ssh/sshd\_config. As the root user update the SSHD config file and update the field PermitRootLogin to No, like this.
* Also, now that we have enabled passwordless authentication, it's a good idea to disable password-based authentication completely and only make use of private key to access the server.
* To do this, change the value of the password authentication field to No, like this. Once done, save and exit the file, and then restart the SSHD service using system's ETL restart SSHD command.
* The root account login and password-based login to the server should not be disabled.
* For more details, you can also refer to Section 5.2 of the CIS Benchmarks for Distribution Independent Linux. It covers a number of security best practices to be considered while hardening the SSH service.
