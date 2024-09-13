# Article: Ingress

As we already discussed **Ingress** in our previous lecture. Here is an update.&#x20;

In this article, we will see what changes have been made in previous and current versions of **Ingress**.

Like in **apiVersion**, **serviceName** and **servicePort** etc.

[![](https://kodekloud.com/kk-media/image/upload/v1698321206/1200736109541070.InaagGGYE8f31Jm2PTKH\_height640.png)](https://kodekloud.com/kk-media/image/upload/v1702469282/course-resource-new/1200736109541070.InaagGGYE8f31Jm2PTKH\_height640.png)    &#x20;

Now, in k8s version **1.20+,** we can create an Ingress resource in the imperative way like this:-

```
Format - kubectl create ingress  --rule="host/path=service:port"
```

```
Example - kubectl create ingress ingress-test --rule="wear.my-online-store.com/wear*=wear-service:80"
```

Find more information and examples in the below reference link:-

[https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-ingress-em-](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-ingress-em-)&#x20;

**References:-**

[https://kubernetes.io/docs/concepts/services-networking/ingress](https://kubernetes.io/docs/concepts/services-networking/ingress)

[https://kubernetes.io/docs/concepts/services-networking/ingress/#path-types](https://kubernetes.io/docs/concepts/services-networking/ingress/#path-types)
