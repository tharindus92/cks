# Kubelet Security

In this lecture, we will revisit Kubelet and the different approaches in configuring Kubelets on nodes.

<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

* In the CKA course when we talked about the ship analogy, we discussed that the Kubelet is like the captain on the ship.&#x20;
* They lead all activities on a ship and they're the ones responsible for doing all the paperwork necessary to become part of the cluster, they're the sole point of contact from the mastership, they load or unload containers on the ship as instructed by the scheduler on the master, and they also send back reports at regular intervals on the status of the ship and the containers on them.&#x20;
* Now what if the captain receives instructions from someone else pretending to be from the mastership, to whom should the captain reveal information regarding the cargo on his ship or how many are there, what are their contents, where are they going etcetera?
* It is important that all communications between the mastership or the Kube-apiserver and the captain on the cargo ship or the Kubelet is secure. That is what we will see in this lecture.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* The Kubelet in the Kubernetes worker node registers the node with the Kubernetes cluster. When it receives instructions to load a container or a pod on the node it requests the container runtime engine which may be Docker or any other runtime engine to pull the required image and run an instance, and the Kubelet then continues to monitor the state of the pod and the containers in it and reports the status to the Kube-apiserver on a timely basis.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* That's the role of the Kubelet. We also discussed how to install the Kubelet, so we basically download the Kubelet binary and configure it to run as a service.&#x20;
* Now just to refresh your memory, note that if you use the Kubeadm tool to deploy your cluster, we know that the Kubeadm tool automatically downloads the required binaries and bootstraps the cluster, however, it does not automatically deploy the Kubelet.&#x20;
* You must always manually install the Kubelet on your worker notes, so that's something to note.
* Now before we look into security, it is important to understand some basic details about the flags that are passed in while configuring the service configuration file, and also the different configuration files that are there for configuring the Kubelet. Now while configuring the Kubelet asset service, we see that it has several options configured such as the container runtime or the Kubelet-config file which is used by the Kubelet to authenticate to the Kube-apiserver, the network plugin the version as well as some additional details such as cluster domain, file check frequency cluster DNS and others.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now this is how it was originally but with the release of version 1.10, most of these parameters were moved to another file called the Kubelet config file for ease of deployment and configuration management. The object created within the file is named Kubelet configuration.

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* On the Kubelet service, we pass the path to this file as a command line argument named config.
* Note that within the file, the parameters use a camel case, so all dashes that separate words in the previous implementation are removed and words are written without spaces, and the first letter of each word is capitalized except the first word.
* HTTP-check-frequency becomes HTTP Check Frequency with the C and F capitalized. Now if you specify a flag both in the service configuration file or on the command line as well as in the file in the Kubelet configuration file, then the flag specified on the command line will override whatever is in this file. Just keep that in mind.
* Now we discussed that the Kubeadm tool does not download or install the Kubelet. However, it can help in managing the Kubelet configuration. Let's say there are a large number of worker nodes and instead of manually creating these Kubelet config files in each of those worker nodes, the Kubeadm tool can help in automatically configuring the Kubelet config files on those nodes when you run the kubeadm join command.

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now once the Kubelet is configured, inspecting the Kubelet process on a node shows the different options configured including the path to the Kubelet-config file. In this case, it happens to be at /var/lib/kubelet/config.yaml.&#x20;
* Now, inspecting that file gives us a list of parameters configured for the Kubelet. Now, the reason we discussed about all of these is to get to this point to know how to inspect the current configurations of a Kubelet and these are some parameters that were passed in with the Kubelet service and many of the flags as you can see are within this file called as the Kubelet configuration file.&#x20;
* If you are asked to inspect how a Kubelet is configured, this is what you should do. You should first identify the Kubelet config file location, and then explore the contents of that file and you'll see all the options set for the Kubelet.
* Now let us go over some of the security aspects of Kubelet. How do we make sure that the cubelet only responds to requests from the Kube-apiserver and not anyone else?

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* The Kubelet serves on two ports, port 10250 which is where the Kubelet serves its API server that allows full access, and port 10255 which serves an API that allows unauthenticated unauthorized read-only access.
*   By default, the Kubelet allows anonymous access to its API. If you do a curl to port 10250 at the API pods, you'll be able to see the list of pods running on the node. Similarly, you can access other APIs such as viewing the system logs of the node on which the Kubelet is running by going to the API/logs/syslog.

    <figure><img src="../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
* As you can see, the Kubelet API provides a number of other API curls such as those to inspect the health of the node, those that exposes metrics, those that perform port forwarding or even running containers or exacting into containers.

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Similarly, the service running at ports 10255 provides read-only access to metrics, to any unauthenticated or unauthorized clients.

<figure><img src="../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now as you can imagine, this is of course a big security risk. Anyone that knows the IP address of this host can access these APIs to perform anything that the API server can do such as viewing existing pods, creating new pods, exacting into existing pods or viewing usage metrics and stats and logs, and more.
* How do you prevent that? How do you implement security on the Kubelet?
* Any request that comes to the Kubelet is first authenticated and then authorized. The authentication process decides if the user or the requesting entity has access to the API, and the authorization process decides what areas of the API can the user access, and what operations can they perform?

<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Let's look at authentication first. We saw that if you did a curl to the API server at port 10250, it returns the list of pods. Now that's because by default the Kubelet permits all requests to go through without any kind of authentication.
* Now, these requests are marked as anonymous users part of an unauthenticated group. This behavior can be changed by setting the anonymous auth flag to false in the Kubelet service configuration file.

<figure><img src="../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Of course, this parameter can also be set in the external Kubelet configuration file as we saw before. It's either in one of these places. On the left, you see the service configuration file where the flag is provided as a command-line parameter, and on the right, you see the Kubelet-config.yaml file in which the same parameter is set, it's just set under the authentication section.
* The best practice is to disable anonymous authentication and enable one of the supported authentication mechanisms. What are those supported authentication mechanisms?
* There are two authentication mechanisms, certificate-based authentication, and API bearer token-based authentication. Of this, we will look at the certificates-based authentication approach.
* First, we create a pair of certificates that will be used as the certificates for serving the Kubelet service, and we then provide the CA file using the client-ca-file command-line argument on the Kubelet service with the path to the CA bundle.
* &#x20;As we discussed before, this can also be set using the Kubelet config file with the authentication section and the client CA file parameter in it like this. This is the recommended approach.

<figure><img src="../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now that the certificate is configured here, to access the api you must provide the client certificates to authorize it. If you are doing a curl you now have to pass in this certificate and the associated key to authenticate into the Kubelet. As far as the Kubelet is considered, the Kube API server is a client.
* The Kube API server also has to authenticate to the Kubelet to interact with it. For this, the API server must have the Kubelet client certificate and key configured. That's the flags that you will see in the Kube-apiserver Service Configuration. The ones that are named as kubelet-client-certificate and kubelet-client-key.
* Remember that the Kube-apiserver is itself a server and it has its own set of certificates and authentication configurations. All the other components of the Kubernetes architecture that requires to communicate with the Kube-apiserver must have certificates configured. However, here in this particular instance, we are referring to the Kubelet alone.
* In this particular instance, when the Kube-apiserver tries to reach the Kubelet, the Kube-apiserver is a client of the Kubelet and it must have the Kubelet client certificates and key configured correctly for it to function.
* This is also the approach used by the Kube ADM tool when it tries to secure Kubelet.
* Now if neither of these two authentication mechanisms explicitly reject a request, the default behavior of the Kubelet is to allow the request as anonymous requests with a username of system anonymous and a group of system unauthenticated.

<figure><img src="../.gitbook/assets/image (14) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Just keep that in mind. Now next, let's talk about authorization. Now, once the user gains access to the system, what resources of the Kubelet API can they interact with, and what actions can they perform? The default authorization mode is always allowed, and this allows all access to the API.
* To prevent this, we set the authorization mode to webhook.

<figure><img src="../.gitbook/assets/image (15) (1) (1).png" alt=""><figcaption></figcaption></figure>

* When set to webhook, the Kubelet makes a call to the API server to determine whether each request can be authorized or not. Depending on the result received from the API server, it either rejects or approves that particular request.

<figure><img src="../.gitbook/assets/image (16) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now, we also talked about the metric service running on port 10255. The one with the read-only access that does not require any kind of authentication or authorization mechanisms. Now, this service is enabled when the read-only port flag on the Kubelet service is not set as a default to port 10255 or it is set to some other port.&#x20;
* If this value is set to zero, then the service is disabled. Note that if a Kubelet config file is in use, then this flag has a default value of zero.&#x20;
* It's disabled by default. You'll probably see this disabled on more systems that you see but if you have detected that the read-only port is not set to zero or if you are able to access the metrics API without any kind of authentication or authorization mechanisms, then it's very likely that this read-only port is set to a value other than zero.
* You can check both of these files, the Kubelet service file, or the Kubelet configuration file, and look for this particular property and have a value set for it to zero.

<figure><img src="../.gitbook/assets/image (18) (1).png" alt=""><figcaption></figcaption></figure>

* Let's summarise what we have learned. The Kubelet, by default, allows anonymous authentication. To prevent it, we set the anonymous flag to false. All of these settings can be done either in the command line service configuration file or the recommended approach is to set them in the Kubelet configuration file.&#x20;
* Kubelet supports two authentication mechanisms, the certificate-based authentication, and the bearer token-based authentication. For certificate-based authentication, set the clientCAfile path to the Kubelet. By default, the Kubelet allows all requests without authorization.&#x20;
* To configure authorization, set the authorization mode to webhook. When this is set, the Kubelet authorizes a request through the Kube-apiserver.&#x20;
* By default, the read-only port is set to 10255, which enables the metric server to expose metrics and other vulnerable information. This can be disabled by setting it to zero.

