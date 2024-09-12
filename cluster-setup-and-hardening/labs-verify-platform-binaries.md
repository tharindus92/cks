# Labs - Verify platform binaries



{% hint style="info" %}
Download Kubernetes source code binary for version `v1.30.0` and place it in the directory - `/opt/`

`=====================`



You will get a download URL from `https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.30.md#v1300`.

Then you can run `wget` command like below: -

```sh
wget -O /opt/kubernetes.tar.gz https://dl.k8s.io/v1.30.0/kubernetes.tar.gz
```
{% endhint %}



{% hint style="info" %}
What are first `10` characters of the shasum of previously downloaded file?

Use `sha512` hash mechanism

\======================

Run the command: `shasum -a512 /opt/kubernetes.tar.gz` or `sha512sum /opt/kubernetes.tar.gz`
{% endhint %}



{% hint style="info" %}
Now lets see how shasum changes if we decompress and compress the same package with some modification

\


Decompress the `/opt/kubernetes.tar.gz` in the same directory and modify file named `version` with some random message and then compress the directory back with name `/opt/kubernetes-modified.tar.gz`.

Now compare `shasum` of a new file with the original.

\=====================



Run below

```
cd /opt/
tar -xf kubernetes.tar.gz
cd kubernetes
echo "v1.30.0-modified" > version
cd ..
tar -czf kubernetes-modified.tar.gz kubernetes
shasum -a512 kubernetes-modified.tar.gz
```
{% endhint %}



{% hint style="info" %}
_info_

To summarize: The shasum of a file changes when its contents are modified and should always be compared against the hash on the official pages to ensure the same file is downloaded.
{% endhint %}
