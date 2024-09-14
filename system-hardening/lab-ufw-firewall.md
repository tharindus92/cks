# Lab - UFW Firewall



{% hint style="info" %}
Which of the following commands can be used to display the rules along with rule numbers next to each rule?

\===============

The command `ufw status numbered` command shows firewall status as numbered list of RULES
{% endhint %}



{% hint style="info" %}
What is the command to allow a `tcp` port range between `1000 and 2000` in ufw?

\===============

Run: `ufw allow 1000:2000/tcp`
{% endhint %}



{% hint style="info" %}
How can you reset ufw rules to their default settings?

\==============

`ufw reset` resets the firewall to the default configuration.
{% endhint %}



{% hint style="info" %}
On the `node01` host, add a rule to allow incoming SSH connections.



Do not enable the firewall yet.

\==================

Run: `ufw allow 22` on node01
{% endhint %}



{% hint style="info" %}
We have some services on `node01` host, which are running on tcp port `9090` and `9091`. Add ufw rules to allow incoming connection on these ports from IP range `135.22.65.0/24` to any interface on node01.\


Once this is done, enable the firewall.

\==================

Run the commands: `ufw allow from 135.22.65.0/24 to any port 9090 proto tcp` and `ufw allow from 135.22.65.0/24 to any port 9091 proto tcp` on `node01`\
To enable the firewall: `ufw enable`
{% endhint %}



{% hint style="info" %}
There is a `Lighttpd` service running on the node01. Identify which port it is bound to.

\===========

First check the service and check if it is running:\
\


```sh
root@node01:~# systemctl status lighttpd
● lighttpd.service - Lighttpd Daemon
     Loaded: loaded (/lib/systemd/system/lighttpd.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2023-05-27 07:26:12 EDT; 10min ago
    Process: 13000 ExecStartPre=/usr/sbin/lighttpd -tt -f /etc/lighttpd/lighttpd.conf (code=exited, status=0/SUCCESS)
   Main PID: 13003 (lighttpd)
      Tasks: 1 (limit: 77091)
     Memory: 2.2M
     CGroup: /system.slice/lighttpd.service
             └─13003 /usr/sbin/lighttpd -D -f /etc/lighttpd/lighttpd.conf


root@node01:~#  
```

\


Next, use `netstat` to find the port used by this process:\
\


```sh
root@node01:~# netstat -natulp | grep lighttpd 
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      13003/lighttpd

root@node01:~# 
```

\


We can see that it is bound to port `80`.
{% endhint %}



{% hint style="info" %}
This service was identified to have several vulnerabilities in it. Disable the port `80` on node01 for ALL incoming requests.

\===============

Run: `ufw deny 80` on node01.
{% endhint %}



{% hint style="info" %}
We want to temporarily disable `ufw` firewall on `node01` host but later we will enable it back so make sure to disable the firewall but preserve all rules so that same can be effective when we enable it back.

\=================

run `ufw disable`. This will temporarily disable the firewall but the old rules are still maintained.
{% endhint %}
