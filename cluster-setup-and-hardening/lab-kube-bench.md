# Lab - Kube-bench



{% hint style="info" %}
3. Install kube bench in `/root` directory\


version : `v0.4.0`

Download file: `kube-bench_0.4.0_linux_amd64.tar.gz`

Download `0.4.0` binary and follow the user guide at [https://github.com/aquasecurity/kube-bench/blob/main/docs/installation.md#download-and-install-binaries](https://github.com/aquasecurity/kube-bench/blob/main/docs/installation.md#download-and-install-binaries)

```
curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.4.0/kube-bench_0.4.0_linux_amd64.tar.gz -o kube-bench_0.4.0_linux_amd64.tar.gz
tar -xvf kube-bench_0.4.0_linux_amd64.tar.gz
```
{% endhint %}



{% hint style="info" %}
Run a kube-bench test now and see the results

Run below command to run kube bench

```
./kube-bench --config-dir `pwd`/cfg --config `pwd`/cfg/config.yaml
```
{% endhint %}

