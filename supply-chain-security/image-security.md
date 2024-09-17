# Image Security

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

* In this lecture, we will talk about securing images. We will start with the basics of image names and then work our way towards secure image repositories and how to configure your pods to use images from secure repositories. We deployed a number of different kinds of pods hosting different kinds of applications throughout this course, like web apps and databases and Redis, cache, et cetera. Let’s start with a simple pod definition file for instance. Here, we have used the nginx image to deploy an nginx container.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

* Let’s take a closer look at this image name. The name is nginx, but what is this image and where is this image pull from? This name follows Docker’s image naming convention. Nginx here is the image or the repository name. -When you say nginx, it’s actually library/nginx. The first part stands for the user or the account name. If you don’t provide a user or account name, it assumes it to be library. Library is the name of the default account where Docker’s official images are stored. These images promote best practices and are maintained by a dedicated team who are responsible for reviewing and publishing these official images. Now, if you were to create your own account and create your own repositories or images under it, then you would use a similar pattern. Instead of library, it would be your name or your company’s name.

&#x20;

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* Now, where are these images stored and pulled from? Since we have not specified the location where these images are to be pulled from, it is assumed to be Docker’s default registry, Docker Hub. The DNS name for which is docker.io.&#x20;
* The registry is where all the images are stored. Whenever you create a new image or update an image, you push it to the registry, and every time anyone deploys this application, it is pulled from the registry. There are many other popular registries as well.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* Google’s registry is at gcr.io, where a lot of Kubernetes-related images are stored, like the ones used for performing end-to-end tests on the cluster. These are all publicly accessible images that anyone can download and access. When you have applications built in-house that shouldn’t be made available to the public, hosting an internal private registry may be a good solution. Many cloud service providers such as AWS, Azure, or GCP, provide a private registry by default.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* On any of these solutions, be it on Docker Hub or Google’s registry, or your internal private registry, you may choose to make a repository private so that it can be accessed using a set of credentials. From Docker’s perspective, to run a container using a private image, you first log in to your private registry using the Docker login command. Input your credentials. Once successful, run the application using the image from the private registry.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* Going back to our pod definition file, to use an image from our private registry, we replace the image name with the full path to the one in the private registry.&#x20;
* How do we implement the authentication, the login part? How does Kubernetes get the credentials to access the private registry? Within Kubernetes, we know that the images are pulled and run by the Docker runtime on the worker nodes.&#x20;
* How do you pass the credentials to the Docker runtime on the worker nodes? For that, we first create a secret object with the credentials in it.&#x20;
* The secret is of type Docker registry, and we name it regcred. Docker registry is a built-in secret type that was built for storing Docker credentials. We then specify the registry server name, the user name to access the registry, the password, and the email address of the user.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

* We then specify the secret inside our pod definition file under the image pull secret section. When the pod is created, Kubernetes or the kubelets on the worker node uses the credentials from the secret to pull images. Well, that’s it for this lecture.
