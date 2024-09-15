# Lab - Pod Security Admission.



{% hint style="info" %}
info By default, PodSecurity admission is enabled in your cluster. To verify this configuration, you can use the following command:

kubectl exec -n kube-system kube-apiserver-controlplane\
\-- kube-apiserver -h | grep enable-admission
{% endhint %}



{% hint style="info" %}
To enable Pod Security at the namespace level, you need to add pod security labels to the namespace using the following format:

```
pod-security.kubernetes.io/<MODE>: <LEVEL> 
```

There are different s of Pod security standards. Identify the option that **does not represent** a valid Pod security standard from the following list.

\====================


{% endhint %}



{% hint style="info" %}
We want to apply pod security on namespace `alpha`. To achieve that, add the following label to the namespace `alpha`

```
pod-security.kubernetes.io/warn=baseline
```

\
Note: Namespace `alpha` is already created.

\========================



* You can use the following command to add label to the namespace.

```
kubectl label ns alpha \
  pod-security.kubernetes.io/warn=baseline
```
{% endhint %}



{% hint style="info" %}
We have provided a manifest `baseline-pod.yaml` at the `/root` location of the lab terminal.

Inspect it and create the pod using the manifest in the `alpha` namespace.\


Note: You might see some warnings while applying manifest. Ignore them for now; we will cover them in next questions.

\===============



* You can use the following command to apply the manifest.

```
kubectl apply -n alpha -f /root/baseline-pod.yaml
```
{% endhint %}



{% hint style="info" %}
While applying the manifest `baseline-pod.yaml` in the previous question, you would have seen a warning message as follows:

```
Warning: would violate PodSecurity "baseline:latest": 
privileged (container "baseline-pod" must not set securityContext.privileged=true)
```

The warning occurred because we have defined a `securityContext.privileged=true` in the pod definition, which violates the Pod Security standard of the `alpha` namespace (`pod-security.kubernetes.io/warn=baseline`).
{% endhint %}



{% hint style="info" %}
How can a cluster administrator specify the configuration file path for the admission configuration resource in the API server?

\===============

In a Kubernetes cluster, the cluster administrator can specify the configuration file path for the admission configuration resource in the API server using the `--admission-control-config-file` flag.

This flag allows administrators to provide a file path that contains the configuration for admission controllers.
{% endhint %}



{% hint style="info" %}
We can also use multiple pod security standards together for a single namespace.

For this step, label the namespace `beta` with the `Baseline: enforce` and `Restricted: warn`.

\=============



* Use the following command to apply the labels to the namespace `beta`.

```
kubectl label ns beta \
pod-security.kubernetes.io/enforce=baseline \
pod-security.kubernetes.io/warn=restricted
```
{% endhint %}



{% hint style="info" %}
We have provided a manifest `multi-psa.yaml` at the `/root` location of the lab terminal.

Inspect it and create the pod using the manifest in the `beta` namespace.\


Note: You might see some warnings while applying manifest. It is expected.

\================

* You can use the following command to apply the manifest.

```
kubectl apply -n beta -f /root/multi-psa.yaml 
```
{% endhint %}



{% hint style="info" %}
While applying the manifest `multi-psa.yaml` in the previous question, you would have seen a warning message as follows:

```
Warning: would violate PodSecurity "restricted:latest": allowPrivilegeEscalation != false (container "multi-psa" must set securityContext.allowPrivilegeEscalation=false), 
unrestricted capabilities (container "multi-psa" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "multi-psa" must set securityContext.runAsNonRoot=true), 
runAsUser=0 (container "multi-psa" must not set runAsUser=0), seccompProfile (pod or container "multi-psa" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
```

Pod will be created as it does not violate the `baseline` security standard but it does violate the `restricted` standard.

Also, it will be created despite violating the restricted standard because the restricted standard is in `warn` mode. In this mode, although the pod does not adhere to the restricted standard, it is allowed to be created, and a warning message is issued during the pod creation process.
{% endhint %}



{% hint style="info" %}
We have provided an AdmissionConfiguration resource manifest at the `/root` location with name `admission-configuration.yaml`.

Inspect the manifest file and select the correct statement on enforced policies and the restricted levels in the provided AdmissionConfiguration resource.

\================



* Baseline: Enforced, Restricted: Auditing and Warning
{% endhint %}



{% hint style="info" %}
We have provided an AdmissionConfiguration resource manifest at the `/root` location with name `admission-configuration.yaml`.

Which namespace is exempt from the policy enforced by the provided AdmissionConfiguration resource?

\=================

* In the `exemptions` section of the manifest, you can see that namespace `my-namespace` is exempted from policy.
{% endhint %}
