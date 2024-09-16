# Security Contexts

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Hello and welcome to this lecture on Security Contexts in Kubernetes. My name is Mumshad Mannambeth. As we saw in the previous lecture, when you run a docker container, you have the option to define a set of security standards such as the ID of the user used to run the container, the Linux capabilities that can be added or removed from the container, et cetera.

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

* These can be configured in Kubernetes as well. As you know already, in Kubernetes, containers are encapsulated in pods. You may choose to configure the security settings at a container level or a pod level. If you configure it at a pod level, the settings will carry over to all the containers within the pod. If you configure at both the pod and the container, the settings on the container will override the settings on the pod.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

* Let us start with a pod definition file. This pod runs an ubuntu image with the sleep command. To configure security context on the container, add a field called securityContext under the spec section of the pod. Use the runAsUser option to set the User ID for the pod.

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

* To set the same configuration on the container level, move the whole section under the container specification like this.

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

* To add capabilities, use the capabilities option and specify a list of capabilities to add to the pod. Well, that's all on Security Contexts in Kubernetes.
