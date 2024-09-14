# Lab - Identify open ports, remove packages services



{% hint style="info" %}
Which of the following commands is used to list all installed packages on an ubuntu system?

\=============

To print only the installed packages, use the `--installed` flag with the `apt list` command.
{% endhint %}



{% hint style="info" %}
Which of the following commands can be used to list only active services on a system?

\=============

The `--all` flag with `systemctl list-units` prints `ALL` services, both active and inactive.\
The `--state inactive` flag when added to this command will print only the inactive servies.\
\
The correct option is `systemctl list-units --type service`
{% endhint %}



{% hint style="info" %}
On the `controlplane` host, we have `nginx` service running which isn't needed on that system. Stop the `nginx` service and remove its service unit file. Make sure not to remove `nginx` package from the system.

\=======

Find out the name of the unit file:

systemctl list-units --all | grep nginx

Stop Nginx service:

systemctl stop nginx

Find out the location of the service unit:

systemctl status nginx

Remove the unit file:

rm /lib/systemd/system/nginx.service
{% endhint %}



{% hint style="info" %}
We want to blacklist the `evbug` kernel module on `controlplane` host.\
`Important Note`: Do not reboot the node!

\=================

Open `vim /etc/modprobe.d/blacklist.conf` file and

Change:

`#blacklist evbug`

To

`blacklist evbug`
{% endhint %}



{% hint style="info" %}
Remove the `nginx` package from `controlplane` host.

\======

apt remove nginx -y
{% endhint %}



{% hint style="info" %}
We have a service running on `controlplane` host which is listening on port `9090`. Identify the service and stop the same to free the `9090` port.

\========

Identify the service listening on port `9090`

`netstat -natp | grep 9090`

Kill/Stop the service to free the port

`systemctl stop apache2`
{% endhint %}



{% hint style="info" %}
We have the `wget` package version `v1.18` installed on the host `controlplane`. There were issues reported with the current version of `wget`. Please check for updates available for this package and update to the latest version available in the apt repos.

\================

apt install wget -y
{% endhint %}
