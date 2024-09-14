# Lab - AppArmor



{% hint style="info" %}
Which state is `AppArmor` in as of the given Kubernetes version?

\===========

beta
{% endhint %}



{% hint style="info" %}
Is the AppArmor module loaded in `controlplane` node?

\=============

yes
{% endhint %}



{% hint style="info" %}
How many AppArmor profiles have been loaded in total currently?

\=========

Run the `aa-status` command to check.
{% endhint %}



{% hint style="info" %}
What is the name of the `AppArmor` profile used by this pod?

\=============


{% endhint %}



{% hint style="info" %}
Load the AppArmor profile called `custom-nginx` on `controlplane` node and make sure that it is in enforced mode.

The profile file is called `usr.sbin.nginx` located in the `default` AppArmor profiles directory.

\==============

Run: `apparmor_parser -q /etc/apparmor.d/usr.sbin.nginx`
{% endhint %}



{% hint style="info" %}
If the previous task was completed, you should see the `nginx` pod now transition from `Error` to a `Running` state.\

{% endhint %}



{% hint style="info" %}
This custom `nginx` pod serves static web pages at two urls.\
1\. `Public Site` can be accessed by clicking on the `Site` link above the terminal and adding `/allowed/` to the end of the URL.\
\
2\. `Restricted Site` may be opened by opening the same `Site` link and adding `/restricted/` to the end of the URL.\
\
**NOTE**: If you see a 404, error, make sure to use `https` to access the web pages.

Which tabs are you able to access?
{% endhint %}



{% hint style="info" %}
Let's fix that. Another profile is created at `/etc/apparmor.d/usr.sbin.nginx-updated` which prevents reads on the restricted directory inside the container.\


Use this AppArmor profile and recreate this container.

\======================

The profile in this file is called `restricted-nginx`. First, make sure it is loaded using the `aa-status` command.\
If not, run the `apparmor_parser -q /etc/apparmor.d/usr.sbin.nginx-updated` and validate the `aa-status` command again.\
Next, update the pod YAML file's annotation with the `restricted-nginx` apparmor profile and then recreate the pod.
{% endhint %}



{% hint style="info" %}
_nfo_

Try accessing the `Public Site` and the `Restricted Site` URLS again now (click on the `Site` button and append `/allowed/` and `/restricted/`. Only the allowed site should work.

Inspect the new AppArmor profile `restricted-nginx` that was used for this pod. If you compare this to the original profile, you will see that the updated profile denies reads on `/usr/share/nginx/html/restricted`
{% endhint %}
