# Kubernetes Software Versions

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

* Hello and welcome to this lecture. In this lecture we will discuss about the various Kubernetes releases and versions. So what do we know about API versions in Kubernetes so far? We know that when we install a kubernetes cluster, we install a specific version of kubernetes.&#x20;
* We can see that when we run the kubectl get nodes command. In this case its v1.11.3. In this lecture we will see how kubernetes project manages software releases. Let's take a closer look at that version number.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* The kubernetes release versions consists of 3 parts. The first is the major version, followed by the minor version and then the Patch version. While minor versions are released every few months with new features and functionalities, patches are released more often with critical bug fixes. Just like many other popular applications out there, kubernetes follows a standard software release versioning procedure.&#x20;
* Every few month It comes out with new features and functionalities through a minor release. The first major version 1.0 was released in July of 2015. As of this recording the latest stable version is 1.13.0.&#x20;
* Whatever we have seen here are stable releases of kubernetes. Apart from this you will also see alpha and beta releases all the bug fixes and improvements first go into an alpha release tagged alpha in this release.&#x20;
* The features are disabled by default and maybe buggy. Then from there they make their way to beta release where the code is well tested. The new features are enabled by default. And finally they make their way to the main stable release.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* You can find all the releases in the releases page of the Kubernetes Github repository. Download the kubernetes.tar.gz file and extract it to find executables for all the kubernetes components. The downloaded package when extracted has all the control plane components in it.&#x20;
* All of them of the same version. Remember that there are other components within the control plane that do not have the same version numbers.&#x20;
* The ETCD cluster and CoreDNS servers have their own versions as they are separate projects. The release notes of each release provides information about the supported versions of externally dependent applications like ETCD and CoreDNS etc.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
