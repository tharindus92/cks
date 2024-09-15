# Pod Security Admission and Pod Security Standards

<figure><img src="../.gitbook/assets/image (231).png" alt=""><figcaption></figcaption></figure>

* In this lecture, we'll take a look at Pod Security Admission and Pod Security Standards. So, a Kubernetes enhancement proposal KEP 2579 was drafted to build a replacement for PSP that are safe to enable in new clusters, easy to use, and extensible, these being a few of the multiple requirements that were furnished in the KEP.

<figure><img src="../.gitbook/assets/image (233).png" alt=""><figcaption></figcaption></figure>

* The new implementation had to be safe and easy to use, and anything that requires complications must go to external solutions like k-rail, Kyverno, or OPA gatekeepers, which we'll discuss in the later sections of this course.

<figure><img src="../.gitbook/assets/image (234).png" alt=""><figcaption></figcaption></figure>

* So, today, Pod Security Admission is also similar to the Pod Security Policy's admission controller. It's an admission controller and is enabled by default, so you don't have to perform any extra steps to enable it.&#x20;
* To verify if it's enabled or not, run the kube-apiserver -h command within the kube-apiserver pod and grep for pod security, and you should be able to see that it is one of the admission controllers that are enabled by default.

<figure><img src="../.gitbook/assets/image (235).png" alt=""><figcaption></figcaption></figure>

* So, PSA is configured at a namespace level, and you do that by applying a label to the namespace. So, to apply pod security standards to the namespace payroll, let's say, using a label, we run the command kubectl label namespace payroll, and use the label pod-security.kubernetes.io, and we provide a mode and policy standard. We will look into what the mode and policy standards are in a bit.
* So, the same way to apply to other namespaces, we're on the same command targeting other namespaces, and we kind of provide this mode and security standards.
* So, what are the mode and policy standards?

<figure><img src="../.gitbook/assets/image (236).png" alt=""><figcaption></figcaption></figure>

* So, remember that one of the goals of PSAs was to simplify the whole implementation.&#x20;
* So, instead of requiring users to write their own profiles, a few built-in profiles are defined that let you choose what standard to apply. So, there are three profiles available: privileged, baseline, and restricted. Privileged means there's unrestricted policy, meaning it allows the widest possible level of permissions, almost like there are no restrictions in place and allows for known privilege escalations. Restricted, on the other end, is the heavily restricted policy and follows the pod hardening best practices.&#x20;
* And baseline is somewhere in the middle that is minimally restricted.
* &#x20;So, you can choose one of these options while configuring pod security for a given namespace. A mode defines what action the control plane takes if the policy is violated. So, if it's set to Enforce, then the pod creation request is rejected.&#x20;
* And if it's set to Audit, then the pod creation is allowed and an entry is added to the audit logs. And Warn triggers a user facing a warning.
* &#x20;And we can use these in any combination we like. For example, you could say enforce restricted policy. That would mean the pod creation would be heavily restricted.&#x20;
* And since it's enforced, anything that violates these policies that are defined within restricted would just be rejected.
* &#x20;But, let's say, if you do warn and restricted, then that would mean users would be able to create any pods, even if they violate the restricted policies, but they're just going to get a warning that says that is not allowed.

<figure><img src="../.gitbook/assets/image (237).png" alt=""><figcaption></figcaption></figure>

* So, just to give you an idea of what these policies are, for example, the baseline policy is designed to be easy to use for most container-based applications, and it helps stop users from gaining unauthorized access to higher privileges. So, the specific restrictions can be seen from the documentation, Kubernetes documentation pages as shown here.

<figure><img src="../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

* And the restricted policy focuses on applying the latest best security measures for pods. And this might cause some compatibility issues, but it ensures stronger security. And you can see the specific restrictions on the Kubernetes documentation pages. And, and the first one that we discussed about, which is privileged, doesn't really have any restrictions.

<figure><img src="../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

* So, back to our namespaces, I'd like to enforce restricted policy on the payroll system, meaning it'll be heavily restricted, and any pod with specs violating the restrictions will be rejected. So, I'm going to apply the enforce mode and restricted profile for it. And for hr, I'm going to enforce baseline restrictions. But for Dev, I'm going warn the user whenever a policy is violated. So, we use the warn mode and the restricted profile. So, this way, it doesn't really prevent the creation but only shows a warning.

