# Kubectl Proxy & Port Forward

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* We learned in the CKA course that we can use the Kubectl utility to interact with the Kubernetes API server. When doing so, we don't need to provide any authentication mechanism in the command line because we configured it in the Kube config file on our system.
* We learned that the Kube config file has the necessary user details and credentials in it to access and interact with the Kubernetes cluster through the API server.&#x20;
* Now the Kubectl utility could be anywhere. It may be on the same host as the master node in the Kubernetes cluster as this in our labs. It may be elsewhere, say, on our laptop.&#x20;
* The cluster could be within a VM on my laptop or on a private server in the environment, or a server on the public cloud environment, or it could be from some managed Kubernetes service provider.
* No matter where the cluster is hosted, you can manage it locally using the Kubectl utility from your laptop as long as you have the Kube config file with the necessary security credentials.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* We also discuss that another way to interact with the Kubernetes API server is through the port 6443 using curl.
* Say, if you were to access the API server directly through using curl as shown here, then you will not be allowed access as you have not specified any authentication mechanisms. You have to authenticate to the API server using your certificate files by passing them in the command line like this.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* An alternate option is to start a Kubectl proxy client. The Kubectl proxy client launches a proxy service locally on port 8001 by default, and uses the credentials and certificates from your Kube config file to access the cluster.
* That way, you don't have to specify those in the curl command when you try to access the API server. Now, when you access the Kubectl proxy server is on local host at port 8001, the proxy will use the credentials from the Kube config file stored on your local computer and forward your request to the Kube API server.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* This will list all available APIs at route. This is an alternate way of accessing the Kube API server by using the Kubectl proxy, without having to specify any security configurations in the curl command.
* Remember that the proxy runs only on your laptop, and is only accessible within your laptop. By default, it accepts traffic from the loopback address at 127.0.0.1 only.&#x20;
* It's not accessible from outside of your laptop.
* What all can you do with this Kubectl proxy? You can make any kind of API requests to the API server with this, and not just that, you can proxy your request to any service that's running within the Kubernetes cluster.
* Say, for example, we have nginx pod exposed with an nginx service, but say for some security reason, we have not exposed it to outside of the cluster through a node port or a load balancer. The nginx service is a cluster IP type service that is only accessible within the cluster.

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Now, what if you want to reach that nginx service from your laptop as an administrator? The only way to do that would be to expose it as a node port or a load balancer port, but you don't want to do that.
* &#x20;One option to access that particular service that's running within the cluster is, again, using this Kubectl proxy. You can access the Kubectl proxy URL, and then form a URL through your Kubectl proxy at port 8001, that is to the /api/v1/namespaces, and then provide the namespace of the application that it is in in this case, it's in the default namespace, so we use default, and that is followed by the API path called services, and then the name of the service that you would like to access followed by what's called the keyword proxy or the path proxy.
* This way you will be able to access services hosted within your cluster through the Kubectl proxy that's running on your laptop as if the cluster and the services are just running locally on your laptop.
* Another option to access the service is to configure a port forward. With Kubectl, you could forward a port from your laptop to a port on a service within the Kubernetes cluster.

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

* The Kubectl port forward command takes a pod, deployment, replicaset, or a service as an argument. In this case, the service nginx is what we want to forward traffic to. Then we specify a port on our host, which is 28080, and that is forwarded to port 80 on the service, which is the nginx service running within the Kubernetes cluster. Now to access the service running on the remote cluster, we can just do a curl to localhost to port 28080, and any request to that particular port is going to be forwarded to the service running within the Kubernetes cluster.&#x20;
* We can use this approach to access any service that is hosted within the cluster so that you can access it as if it were running now on your local host or on your laptop, even though the actual cluster could be miles away, probably in some cloud environment.
* These are different ways that we can run a local proxy using Kubectl utility to access a service, or the API server itself that's running on a remote Kubernetes cluster. Now, the reason we discuss these approaches is because this will come in handy in the upcoming lectures where we discuss about Kubernetes dashboard.
