# Cluster Upgrade Process

<figure><img src="../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

* The components can be at differen release version
* components can be at different release versions since the Kube API server is the primary component in the control plane and that is the component that all other components talked to.&#x20;
* None of the other components should ever be at a version higher than the Kube API server the controller manager and scheduler can be at one version lower.&#x20;
* So if Kube API server was at X control or managers and Kube schedulers can be at X minus one and the Kubelet and Kube proxy components can be at two versions lower X minus two.
* &#x20;So if Kube API server was at 1.10 the controller manager and scheduler could be at 1.10 or 1.9 and the Kubelet and Kube proxy could be at 1.8 none of them could be at a version higher than the Kube API server like 1.11.
* Now this is not the case with Kube control the Kube control utility could be at 1.11 it wasn't higher than the API server 1.10 the same version as the API server or at 1.9 inversion lower than the API server now this permissible SKU inversions allows us to carry out life upgrades we can upgrade component by component if required.

<figure><img src="../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

* So when should you upgrade, say you were at 1.10 and kubernets releases versions 1. 11 and 1.12.  at any time  kubernetes support only up to the recent three minor versions.
* So with 1.12 being the latest release kubernets supports versions 1.12,1.11 and 1.10.
* So when 1.13 is released only versions 1.13,1.12and 1.0.11 are supported.
* Before the release of 1.13 would be a good time to upgrade your cluster to the next release.
* supported for 3 minor version
* So how do we upgrade do we upgrade directly from 1.10 to under 13. No.The recommended approach is to upgrade one minor version at a time.
* Version 1.10 to 1.11 then 1.11 to 1.0 12. And then 1.12 to under 1.13.

<figure><img src="../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

* The upgrade process depends on how your cluster is setup. For example if your cluster is a managed kubernets cluster deployed on cloud service providers like Google for instance kubernets engine lets you upgrade your cluster easily with just a few clicks.
* If you deployed your cluster using tools like kubeadm then the tool can help you plan and upgrade the cluster.
* If you deployed your cluster from scratch then you manually upgrade the different components of the cluster yourself.
* In this lecture we will look at the options by kubeadm.

## Upgrade with kubeadm

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

* Firstupgrade master node and then upgrade the worker nodes.
* while the master is being upgraded, the control plane component such as API server,scheduler and controller-managers, go down briefly.
* when master node goes down does not mean your worke nodes and application on the clustert are impacted. All workload hosted on the worker nodes continue to seve users as normal.
* All management funtions are down
* you cannt access the cluster using kubectl or otherkubenetes API
* You cannot deploy new applications,delet or modify exiting ones.
* The controller manager don't funtion either. If a pod was to fail, a new pod won't be automaticlly created.
* But as long as the nodes and th pods are up,your application should be up and users will not be impacted.
* Once the upgrade is complete and the cluster is back up it should function normally.
* We now have the master and the master components had version 1.11 and the worker nodes at version 1.10.

<figure><img src="../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

### Worker node upgrade strategy

* As we saw earlier this is a supported configuration has now time to upgrade the worker nodes. There are different strategies available to upgrade the worker nodes.

<figure><img src="../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

* One is to upgrade all of them at once but then your ports are down and users are no longer able to access the applications.
* Once the upgrade is complete the nodes are backed up new pods are scheduled and users can resume access. That's one strategy that requires downtime.

<figure><img src="../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

* The second strategy is to upgrade one node at a time. So going back to the state where we have our master upgraded and nodes waiting to be upgraded we first upgrade the first node where the workloads move to the second and third node and users are so from there.&#x20;
* Once the first note is upgraded and backed up with an update the second node where the workloads move to the first and third nodes and finally the third node where the workloads are shared between the first two.
* Until we have all nodes upgraded to a newer version, We then follow the same procedure to upgrade the nodes from 1.12 and then 1.13.

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

* A third strategy would be to add new node to the cluster nodes with newer software version. This is especially convenient if you're on a cloud environment where you can easily provision new nodes and decommission old ones nodes with the newer software version can be added to the cluster.

## Master node

<figure><img src="../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

* kubeadm not upgrade the kubelet we have to update it manualy
* 1st we have to upgrde kubem tool with the same software vesion as kubenetes.

<figure><img src="../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>

```
apt-get upgrade -y kubeadm=1.12.0-00
kubeadm upgrade apply v1.12.0
apt-get upgrade -y kubelet=1.12.0-00
systemctl restart kubelet
```

## Worker node

<figure><img src="../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

```
apt-get upgrade -y kubeadm=1.12.0-00
apt-get upgrade -y kubelet=1.12.0-00
kebeadm upgrade node config --kubelet-version v1.1.0
systemctl restart kubelet
```
