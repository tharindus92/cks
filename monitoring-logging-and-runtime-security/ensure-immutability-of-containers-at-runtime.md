# Ensure Immutability of Containers at Runtime

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-060-monitoring-logging-runtime-security_page-0066.jpg" alt=""><figcaption></figcaption></figure>

* In this lecture, let us look at the different ways in which we can ensure that our Kubernetes pods adhere to the concept of immutability. In the previous lecture, we saw that although containers are meant to be immutable by default, we can carry out in-place updates on them.&#x20;
* Now, this can be done in a number of different ways, such as copying files directly into the pod, or getting a shell into the container and making changes. How can we prevent this?

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-060-monitoring-logging-runtime-security_page-0067.jpg" alt=""><figcaption></figcaption></figure>

* One way to achieve this is to make sure that we cannot write into the file system once the pod is started. To do this, we can use the security context that we have seen many times earlier in this course. Here, we have the pod definition file for an engine export.&#x20;
* Now using security context at the container level, we can make use of the field called read-only file system and set its value to true like this.&#x20;
* This will ensure that the Nginx container starts with a read-only root file system, which will make it impossible to copy or write inside the container.&#x20;
* However, just adding this field may break the application which is running inside. For example, if we try to create this pod now, it is likely to fail.&#x20;
* This is because the application running inside would probably need to write to certain directories to function properly. For example, Nginx needs to be able to write into two different directories /var/run to store data that Nginx needs to start and /var/cache/nginx for storing cache data.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-060-monitoring-logging-runtime-security_page-0068.jpg" alt=""><figcaption></figcaption></figure>

* We can see these in the pods logs. Because we have set the root file system as read-only Nginx can no longer write into these directories. To fix this, we can make use of volumes to be mounted on those specific directories.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-060-monitoring-logging-runtime-security_page-0069.jpg" alt=""><figcaption></figcaption></figure>

* Now, in this example, this can be an empty directory type volume as we do not want the data stored in these two directories to persist. This will ensure that the directories which are /var/run and /var/cache/nginx on the container can now be written into while the rest of the root file system remains read-only.&#x20;

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-060-monitoring-logging-runtime-security_page-0070.jpg" alt=""><figcaption></figcaption></figure>

* Now, if you recreate the pod, it should start successfully. That is one way to ensure container immutability.
* Now if you try to copy files into the container or updated it in any way, it should fail.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-060-monitoring-logging-runtime-security_page-0071.jpg" alt=""><figcaption></figcaption></figure>

* Before we move on, let us see another example. The same Nginx pod for which we have used the read-only root file system.&#x20;
* However, let us assume that now we are running this pod with the privileged flag set to true.&#x20;
* We have already seen throughout this course why we should not be making use of the privileged flag when running containers, but for argument's sake, let us see what happens if we do. Using this configuration file, let us create the Nginx container as before.&#x20;
* Sure enough, if you try to do an apt update, it will fail as the root file system is still read-only.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-060-monitoring-logging-runtime-security_page-0072.jpg" alt=""><figcaption></figcaption></figure>

* Let us try another thing. Let us check the swappiness value by inspecting the /proc file system. We can see that this is set to the value of 60.&#x20;
* Now, let us see if we can change this value. From the result, we can see that it has been changed to the value of 75. The /proc is a pseudo-file system.&#x20;
* We were able to make changes to a file within this file system, but that's not all. If we inspects the value of vm.swappiness on the host, we can see that it has changed too.
* This is another reason why we should refrain from running containers with high privileges. In this example, although we had used the read-only file system for root, we were still able to make changes on the host machine.

<figure><img src="../.gitbook/assets/KodeKloud-Kubernetes-CKS-060-monitoring-logging-runtime-security_page-0073.jpg" alt=""><figcaption></figcaption></figure>

* The key takeaway here is to make sure we implement root as read-only file system as much as possible and make sure to use volumes for temporary data and external mounts using persistent volumes in cases where we need to use state.&#x20;
* Also using the privilege flag can also break immutability of the containers. As a best practice, do not use the privileged flag and run the containers as non-root users as much as possible.
* &#x20;One way to enforce it is to make use of pod security policies that we have seen earlier in the course. In general, stick to the principle of least privilege and drop any privileges that is not needed by the containers to do their job.
