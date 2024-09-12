# Kubernetes Dashboard

<figure><img src="../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

In this lecture, we will look at the Kubernetes dashboard, and one of the Kubernetes subprojects is a web-based graphical user interface known as the Kubernetes dashboard. It may be used to get a graphical representation of the cluster and monitor various activities, and even provision new resources on the cluster from the same interface.

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

* It's a powerful tool that can do a lot of things such as even view existing configurations, including secrets stored on the cluster. Not just view, but it can also help in creating new applications and deploying them on the cluster.
* As such, it is important that we protect access to it. In the earlier releases of the Kubernetes dashboard, access control was not really restricted, and as such, there have been instances of the dashboard being the target of cyberattacks, and such as the one at Tesla alerted by the RedLock cloud security team.
* The dashboard was made publicly accessible without authentication, which enabled cyberattackers to use the infrastructure for mining cryptocurrencies.

<figure><img src="../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

* We will see how the Kubernetes dashboard is deployed first. Then we will look at how to secure access to it. Now the Kubernetes dashboard is deployed by applying the recommended configuration available at the Kubernetes dashboard, GitHub repository.

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

* The full path to the URL is available in the link shown below. When deployed, a namespace named as kubernetes-dashboard is created within which a bunch of objects are created such as the deployment named kubernetes-dashboard that hosts the dashboard UI server, the service named kubernetes-dashboard that exposes the deployment config maps that store the dashboard settings, and the number of secret objects that stores certificates.
* By default, the service that exposes the dashboard is not fed to load balancer or NodePort. It is set to cluster IP, meaning, by default, the dashboard is not accessible outside of the cluster, and it's only accessible from within the cluster.
* This is done on purpose as we don't want the dashboard to be accessible publicly to the entire world. If the VMs hosting your Kubernetes cluster happens to have a graphical user interface and a browser, which is usually not the case, but if you did, then you could simply open a browser and go to the IP address of the service and access the dashboard, but that's not possible since the servers hosting the cluster usually don't have a GUI.
* How do you access the Kubernetes dashboard hosted on a cluster? What you really want to do is access the dashboard from your browser on your laptop machine.
* On our laptop, we have the kubectl utility and the kubeconfig file that we have been using to interact with the Kubernetes cluster. We learned in the previous lecture on kubectl proxy, that we could create a proxy to access a service within the Kubernetes clusters safely from our laptop.
* For this, we run the kubectl proxy command, which creates a proxy on local host that proxies all requests to the API server on the Kubernetes cluster.
* Then we can now access a service within the cluster through this proxy. Now on your laptop, you could open a browser and go to the URL, localhost port 8001, which is a port of our proxy.

<figure><img src="../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

* Then it's followed by api/v1/namespaces, and the kubernetes-dashboard namespace followed by services, and the name of the service, which is kubernetes-dashboard service. Now that is HTTPS as well, so we specify that.
* Then we finally have the /proxy path. We can now access the Kubernetes dashboard from our laptop.
* I'll talk about the authentication mechanisms in a bit.

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

* Again, remember that this dashboard is only accessible within our laptop, as it is being accessed through the proxy that is running on my laptop.
* &#x20;This approach is not valid for making the dashboard accessible across a team of users. While doing that, you need to exercise extreme caution and make sure that only users with the right privileges have access to it.&#x20;
* The whole reason we had to do this was because the Kubernetes dashboard service has a type set to cluster IP, which is not accessible outside the cluster. First, how do we get around that?

<figure><img src="../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

* Now, one thing that we could do is set its type two load balancer. If the cluster is deployed in a cloud environment, then it makes the dashboard accessible outside of the cluster, the public world, but that is highly discouraged as we don't want our dashboard made public, so don't ever do that.

<figure><img src="../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

* The other option is to set the service type to NodePort. Setting it to NodePort will expose the application on ports of the nodes in the cluster. Now, if you're confident that your network is secure, then this may be a viable option.

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

* Another option is to have an authentication proxy configured such as the auth to proxy. They can perform authentication, and on success, route traffic to the dashboard. However, deploying such an authentication proxy is out of scope for this lecture.

