# Lab - Limit Node Access



{% hint style="info" %}
There is a user named `himanshi` on the `controlplane` host. Suspend this user account so that this user cannot login to the system but make sure not to delete it.

\=========

Run: `usermod -s /usr/sbin/nologin himanshi`. This will make sure that the user can no longer login with their credentials.
{% endhint %}



{% hint style="info" %}
Create a user named `sam` on the `controlplane` host. The user's home directory must be `/opt/sam`. Login shell must be `/bin/bash` and `uid` must be `2328`. Make `sam` a member of the `admin` group.

\========

Run: `useradd -d /opt/sam -s /bin/bash -G admin -u 2328 sam`
{% endhint %}
