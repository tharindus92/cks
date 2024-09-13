# Lab - SSH Hardening and sudo



{% hint style="info" %}
Create a user named `jim` on `node01` host and configure password-less ssh access from `controlplane` host (from user `root`) to `node01` host (to user `jim`).

\=================



* ssh into `node01` host from `controlplane` host

`ssh node01`

* Create user `jim` on `node01` host

`adduser jim` (set any password you like)

* Return back to `controlplane` host and copy ssh public key

`ssh-copy-id -i ~/.ssh/id_rsa.pub jim@node01`

* Test ssh access from `controlplane` host

`ssh jim@node01`
{% endhint %}



{% hint style="info" %}
Change the password of user `jim` to `8DHdjjdk` on host `node01` and make him a `sudo` user.

\=========================

* SSH into `node01` host and reset password for user `jim`

`passwd jim`

* On `node01` host, open `/etc/sudoers` file using any editor like `vi` and add an entry for user jim and forcefully save the file.

`jim ALL=(ALL:ALL) ALL`
{% endhint %}



{% hint style="info" %}
We want to update user `jim` on `node01` host so that jim can run sudo commands without entering the sudo password. Please make appropriate changes.

\===================



* On `node01` host, open `/etc/sudoers` file using any editor like `vi` and edit entry for user jim and forcefully save the file.

Change `jim ALL=(ALL:ALL) ALL`

To `jim ALL=(ALL) NOPASSWD:ALL`
{% endhint %}



{% hint style="info" %}
Add a new user `rob` on `node01` host and set password to `jid345kjf`. We already have a sudo group i.e `admin` on this host. Make user `rob` member of the `admin` group so that he can become a `sudo` user. Do not to add the user directly in the `sudoers` file.

\====================

* SSH into `node01` host and create user `rob`:\
  `adduser rob`
* Make it member of `admin` group

`usermod rob -G admin`
{% endhint %}



{% hint style="info" %}
There is some issue with `sudo` on `node01` host, as user `rob` is not able to run sudo commands, investigate and fix this issue.

Password for user `rob` that we set in the previous question: `jid345kjf`

`=================`

On `node01` host add `%` before `admin` group related entry. It should look like

`%admin ALL=(ALL) ALL`
{% endhint %}



{% hint style="info" %}
Disable ssh root login and disable password authentication for ssh on `node01` host.

\=====================



* On `node01` host open `/etc/ssh/sshd_config` config file using any editor like `vi` and make appropriate changes

Change: `#PermitRootLogin prohibit-password`

To: `PermitRootLogin no`

Change: `#PasswordAuthentication yes`

To: `PasswordAuthentication no`

* Restart `sshd` service by using command:

`service sshd restart`
{% endhint %}
