# Page 1

{% hint style="info" %}
Install and run OPA version `v0.38.1` on the system in the background.\


Package should be `opa_linux_amd64`. Once downloaded, make package **executable** and run it as a **server**.

\==============



Download installer from `https://github.com/open-policy-agent/opa/releases`\
Package: `opa_linux_amd64`

Run below commands:

```
# Find the given version of OPA from the release page.
export VERSION=v0.38.1
curl -L -o opa https://github.com/open-policy-agent/opa/releases/download/${VERSION}/opa_linux_amd64

chmod 755 ./opa

./opa run -s &
```
{% endhint %}



{% hint style="info" %}
A sample policy is given at `/root/example.rego`. However there is an issue with the file. Try to fix the error.

\===========

Set `default allow = false` in example.rego

Use Command `./opa test example.rego` to test policy.
{% endhint %}



{% hint style="info" %}
Load policy `/root/sample.rego` to OPA with the name `samplepolicy`.

\=====



Make a PUT API call to `/v1/policies/`

```
curl -X PUT --data-binary @file.rego http://localhost:8181/v1/policies/policyname

==============
Run Below command to import sample.rego in OPA

curl -X PUT --data-binary @sample.rego http://localhost:8181/v1/policies/samplepolicy
```
{% endhint %}
