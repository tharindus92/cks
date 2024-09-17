# Privilege Escalation in Linux

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Let us now take a look at privilege escalation in Linux. In the previous lecture, we disabled the root user login via SSH. We agreed that using root as a daily driver is a bad idea. However, there will be times where we need to run a command using the root user privileges, such as when we try to install a new software or carrying out maintenance on the host.
* How do we do that, now that the root user login is disabled? The preferred way to run commands as a root user, or a superuser, is to make use of Sudo.
* The Sudo command offers another approach to giving users administrative access.
* When trusted users proceed an administrative command with Sudo, they are prompted for their own password.
* The default configuration for Sudo is defined under /etc/sudoers file.
* This file defines the policies applied by Sudo command, and can be updated using the VI Sudo command like this.
* Only users listed in the /etc/sudoers file can make use of Sudo command for privilege escalation.
* This is an added security measure which prevents users from logging into a system as the root user.
* The administrator can give granular level of access with Sudo. In this case, the user called Mark has complete administrative privileges. However, the user called Sarah can only use Sudo to reboot the system.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Commands executed using Sudo are executed in the user's own shell and not in the root shell.
* As a result, we can eliminate the need for ever having to log in as the root user directly.
* We can do this by setting the no login shell for the root user like this.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Let us now look at the syntax of the sudoers file. All lines that begin with a hash or a pound symbol are comments and are not applied.
* The first field is either the user or the group to which privileges have to be granted. Groups begin with a percentage symbol.
* The second field, which by default is left to all, specifies the host in which the user can make use of privilege escalation.
* In a normal setup, there's just one host, which is the local host, and as a result, the value all implies the localhost in this case. The third field enclosed in brackets implies the users and groups. The user in the first field can run the commands.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* This is normally left to the default value of all, which means that any user can be used to run the commands. The fourth field is the command that you can run. You can specify all here, which means that the user can run any commands without any restrictions. Or you can specify individual commands such as the shutdown -r now, as in the case of user called Sarah.
