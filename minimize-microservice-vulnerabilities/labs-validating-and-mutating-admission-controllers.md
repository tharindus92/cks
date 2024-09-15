# Labs - Validating and Mutating Admission Controllers



{% hint style="info" %}
What is the flow of invocation of admission controllers?

![](<../.gitbook/assets/image (18).png>)
{% endhint %}



{% hint style="info" %}
Create **TLS secret** `webhook-server-tls` for secure webhook communication in `webhook-demo` namespace.\


We have already created below cert and key for webhook server which should be used to create secret.

Certificate : `/root/keys/webhook-server-tls.crt`

Key : `/root/keys/webhook-server-tls.key`

`=============================`

kubectl -n webhook-demo create secret tls webhook-server-tls\
\--cert "/root/keys/webhook-server-tls.crt"\
\--key "/root/keys/webhook-server-tls.key"
{% endhint %}



{% hint style="info" %}
info In previous steps we have deployed demo webhook which does below

* Denies all request for pod to run as root in container if no securityContext is provided.
* If no value is set for runAsNonRoot, a default of true is applied, and the user ID defaults to 1234
* Allow to run containers as root if runAsNonRoot set explicitly to false in the securityContext

In next steps we have added some pod definitions file for each scenario. Deploy those pods with existing definitions file and validate the behaviour of our webhook

\=============


{% endhint %}
