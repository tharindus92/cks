# Minimize host OS footprint Intro

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

* We learned earlier that, when it comes to security, there are several places where the attack can occur. One way to limit the threat and reduce the attack surface is to keep all systems in the cluster in a consistent and simple state. One poorly configured node, if it is part of a cluster, or one that has a significant vulnerability that was never fixed, increases the probability of the whole cluster getting compromised.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

* To reduce the complexity of the systems, and therefore reducing the attack surface, we can make use of the principle of least privilege that we got introduced to in the previous lecture.
