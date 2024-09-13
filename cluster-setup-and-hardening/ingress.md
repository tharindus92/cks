# Ingress

* One of the common questions that students reach out about usually is regarding services and ingress.

<figure><img src="../.gitbook/assets/image (158).png" alt=""><figcaption></figcaption></figure>

What's the difference between the two and when to use what? So we are going to briefly revisit services and work our way towards ingress. We will start with a simple scenario. You are deploying an application on Kubernetes for a company that has an online store selling products. Your application would be available at say, myonlinestore.com.&#x20;

You build the application into a docker image and deploy it on the Kubernetes cluster as a pod in a deployment. Your application needs a database so you deploy a My SQL database as a pod and create a service of type cluster IP called my SQL Service to make it accessible to your application. Your application is now working. To make the application accessible to the outside world, you create another service, this time of type node port, and make your application available on a high port on the nodes in the cluster. In this example, a port 38080, is allocated for the service. The users can now access your application using the URL HTTP://IP of any of your nodes, followed by the port 38080. That setup works and users are able to access the application. Whenever traffic increases, we increase the number of replicas of the pod to handle the additional traffic, and the service takes care of splitting traffic between the pods.

<figure><img src="../.gitbook/assets/image (159).png" alt=""><figcaption></figcaption></figure>

However, if you have deployed a production grade application before you know that there are many more things involved in addition to simply splitting the traffic between the pods. For example, we do not want the users to have to type in IP address every time. So you configure your DNS server to point to the IP of the nodes. Your users can now access your application using the URL myonlinestore.com and port 38080. Now, you don't want your users to have to remember a port number either. However, service node ports can only allocate high numbered ports, which are greater than 30,000.

<figure><img src="../.gitbook/assets/image (160).png" alt=""><figcaption></figcaption></figure>

So you then bring in an additional layer between the DNS server and your cluster, like a proxy server that proxies requests on port 80 to port 38080 on your nodes. You then point your DNS to this server and users can now access your application by simply visiting myonlinestore.com. Now this is if your application is hosted on-prem in your data center..

<figure><img src="../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

Let's take a step back and see what you could do if you were on a public cloud environment like Google Cloud Platform. In that case, instead of creating a service of type node port for your wear application, you could set it to type load balancer. When you do that, Kubernetes would still do everything that it has to do for a node port, which is to provision a high port for the service. But in addition to that, Kubernetes also sends a request to Google Cloud Platform to provision a network load balancer for the service. On receiving the request, G-C-P would then automatically deploy a load balancer configured to route traffic to the service ports on all the nodes and return its information to Kubernetes. The load balancer has an external IP that can be provided to users to access the application. In this case, we set the DNS to point to this IP and users access the application using the URL myonlinestore.com.

<figure><img src="../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

Perfect. Your company's business grows and you now have new services for your customers. For example, a video streaming service. You want your users to be able to access your new video streaming service by going to myonlinestore.com/watch. You'd like to make your old application accessible at myonlinestore.com/wear. Your developers developed the new video streaming application as a completely different application as it has nothing to do with the existing one. However, in order to share the same cluster resources, you deploy the new application as a separate deployment within the same cluster. You create a service called video service of type load balancer. Kubernetes provisions port 38282 for this service, and also provisions a network load balancer on the cloud. The new load balancer has a new IP. Remember, you must pay for each of these load balancers and having many such load balancers can inversely affect your cloud bill. So how do you direct traffic between each of these load balancers based on the URL that the users type in? You need yet another proxy or load balancer that can redirect traffic based on URLs to the different services. Every time you introduce a new service, you have to reconfigure the load balancer. And finally, you also need to enable SSL for your applications so your users can access your application using HTTPS. Where do you configure that? It can be done at different levels, either at the application level itself or at the load balancer or proxy server level. But which one? You don't want your developers to implement it in their application as they would do it in different ways. You want it to be configured in one place with minimal maintenance. Now, that's a lot of different configuration and all of this becomes difficult to manage when your application scales. It requires involving different individuals in different teams. You need to configure your firewall rules for each new service, and it's expensive as well as for each service, a new cloud native load balancer needs to be provisioned. Wouldn't it be nice if you could manage all of that within the Kubernetes cluster and have all that configuration as just another Kubernetes definition file that lives along with the rest of your application deployment files?

<figure><img src="../.gitbook/assets/image (164).png" alt=""><figcaption></figcaption></figure>

* That's where ingress comes in. Ingress helps your users access your application using a single externally accessible URL that you can configure to route to different services within your cluster based on the URL path.&#x20;
* At the same time, implement SSL security as well. Simply put, think of ingress as a layer seven load balancer built in to the Kubernetes cluster that can be configured using native Kubernetes primitives just like any other object in Kubernetes.&#x20;
* Now remember, even with ingress, you still need to expose it to make it accessible outside the cluster. So you still have to either publish it as a node port or with a cloud native load balancer, but that is just a one-time configuration.

<figure><img src="../.gitbook/assets/image (165).png" alt=""><figcaption></figcaption></figure>

Going forward, you're going to perform all your load balancing authentication, SSL and URL-based routing configurations on the ingress controller. So how does it work? What is it? Where is it? How can you see it?

How can you configure it? How does it load balance? How does it implement SSL? Without ingress, how would you do all of this?

I would use a reverse proxy or a load balancing solution like N-G-I-N-X or H-A Proxy or Traefik. I would deploy them on a Kubernetes cluster and configure them to route traffic to other services. The configuration involves defining URL routes, configuring SSL certificates, et cetera. Ingress is implemented by Kubernetes in kind of the same way.

<figure><img src="../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>

* You first deploy a supported solution, which happens to be any of these listed here, and then specify a set of rules to configure ingress.&#x20;
* The solution you deploy is called as an ingress controller, and the set of rules you configure are called as ingress resources.&#x20;
* Ingress resources are created using definition files like the ones we use to create pods, deployments and services earlier in this course. Now remember, a Kubernetes cluster does not come with an ingress controller by default.
* If you set up a cluster following the demos in this course, you won't have an ingress controller built into it. So if you simply create ingress resources and expect them to work, they won't. Let's look at each of these in a bit more detail.

<figure><img src="../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>

* As I mentioned, you do not have an ingress controller on Kubernetes by default, so you must deploy one. What do you deploy?&#x20;
* There are a number of solutions available for ingress, a few of them being G-C-E, which is Google's layer seven HTTP load balancer, N-G-I-N-X, Contour, H-A Proxy, Traefik and Istio.&#x20;
* Out of this, G-C-E and N-G-I-N-X are currently being supported and maintained by the Kubernetes project, and in this lecture we will use N-G-I-N-X as an example.&#x20;
* These ingress controllers are not just another load balancer or N-G-I-N-X server. The load balancer components are just a part of it. The ingress controllers have additional intelligence built into them to monitor the Kubernetes cluster for new definitions or ingress resources and configure the N-G-I-N-X server accordingly.

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

* An N-G-I-N-X controller is deployed as just another deployment in Kubernetes. So we start with a deployment definition file named N-G-I-N-X ingress controller with one replica and a simple pod definition template.&#x20;
* We will label it N-G-I-N-X ingress, and the image used is N-G-I-N-X ingress controller with the right version. Now, this is a special build of N-G-I-N-X built specifically to be used as an ingress controller in Kubernetes, so it has its own set of requirements. Within the image, the N-G-I-N-X program is stored at location N-G-I-N-X ingress controller, so you must pass that as the command to start the N-G-I-N-X controller service.&#x20;
* If you have worked with N-G-I-N-X before, you know that it has a set of configuration options such as the path to store the logs, keep a live threshold, SSL settings, session timeouts, et cetera. In order to decouple these configuration data from the N-G-I-N-X controller image, you must create a config map object and pass that in.&#x20;
* Now remember, the config map object need not have any entries at this point. A blank object will do, but creating one makes it easy for you to modify a configuration setting in the future. You will just have to add it into this config map and not have to worry about modifying the N-G-I-N-X configuration files.

<figure><img src="../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

* You must also pass in two environment variables that carry the pod's name and namespace it is deployed to. The N-G-I-N-X service requires these to read the configuration data from within the pod.

<figure><img src="../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

And finally, specify the ports used by the ingress controller, which happens to be 80 and 443.

<figure><img src="../.gitbook/assets/image (171).png" alt=""><figcaption></figcaption></figure>

* We then need a service to expose the ingress controller to the external world. So we create a service of type node port with the N-G-I-N-X ingress label selector to link the service to the deployment.

<figure><img src="../.gitbook/assets/image (172).png" alt=""><figcaption></figcaption></figure>

* As mentioned before, the ingress controllers have additional intelligence built into them to monitor the Kubernetes cluster for ingress resources and configure the underlying N-G-I-N-X server when something is changed.&#x20;
* But for the ingress controller to do this, it requires a service account with the right set of permissions. For that, we create a service account with the correct roles and role bindings.

<figure><img src="../.gitbook/assets/image (173).png" alt=""><figcaption></figcaption></figure>

* So to summarize, with a deployment of the N-G-I-N-X ingress image, a service to expose it, a config map to feed N-G-I-N-X configuration data and a service account with the right permissions to access all of these objects, we should be ready with an ingress controller in its simplest form.

<figure><img src="../.gitbook/assets/image (174).png" alt=""><figcaption></figcaption></figure>

* Now onto the next part of creating ingress resources. And ingress resource is a set of rules and configurations applied on the ingress controller.&#x20;
* You can configure rules to say simply forward all incoming traffic to a single application or route traffic to different applications based on the URL.&#x20;
* So if a user goes to myonlinestore.com/wear, then route to one of the applications, or if the user visits the watch URL, then route to the video app, et cetera. Or you could route users based on the domain name itself.
* &#x20;For example, if the user visits wear.myonlinestore.com, then route the user to the wear application or else route him to the video app.

<figure><img src="../.gitbook/assets/image (175).png" alt=""><figcaption></figcaption></figure>

* As with any other object, we have API version, kind, metadata and spec. The API version is extensions/V1beta1. Kind is ingress. We will name it ingress-wear.
* &#x20;And under spec, we have backend. So the traffic is of course routed to the application services and not pods directly as you might know already.
* &#x20;The backend section defines where the traffic will be routed to. So if it's a single backend, then you don't really have any rules. You can simply specify the service name and port of the backend wear service.&#x20;
* Create the ingress resource by running the Kube control create command View the created ingress resource by running the Kube control get ingress command.&#x20;
* The new ingress is now created and routes all incoming traffic directly to the wear service.

<figure><img src="../.gitbook/assets/image (176).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (177).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>
