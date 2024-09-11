# Kube-bench



* In this lecture, we will look at evaluating CIOs benchmark for Kubernetes using the Kube-bench tool. The kube-bench tool is an open-source tool from Aqua security that can perform automated assessments to check whether Kubernetes is deployed as per security best practices.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

* In the previous lecture, we saw the list of recommended best practices from CIS for Kubernetes, and that is shown here on the left. On the right is the output from the Kube-bench tool. As you can see, each of the recommendations on the left matches the results on the right.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

* There are different ways to deploy and get started with Kube-bench. It may be deployed as a Docker container, or as a job on a Kubernetes cluster, or just as binaries or compiled from source code.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

* In the upcoming labs, you will identify a stable version of Kube-bench from GitHub, install it on a master node, run an assessment, and review its results. You'll also fix some of the issues following the remediation steps recommended by the tool and verify if those fixes are applied.
