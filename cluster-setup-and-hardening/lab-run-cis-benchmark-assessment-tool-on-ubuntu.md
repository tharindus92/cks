# Lab - Run CIS Benchmark Assessment tool on Ubuntu



{% hint style="info" %}
3.  We have installed the CIS-CAT Pro Assessor tool called Assessor-CLI, under `/root`.

    Please run the assessment with the `Assessor-CLI.sh` script inside `Assessor` directory and generate a report called `index.html` in the output directory `/var/www/html/`.Once done, the report can be viewed using the `Assessment Report` tab located above the terminal.\


    \


    Run the test in interactive mode and use below settings:

    Benchmarks/Data-Stream Collections: : `CIS Ubuntu Linux 20.04 LTS Benchmark v2.0.1`

    Profile : `Level 1 - Server`

Use `sh ./Assessor-CLI.sh` to see all available options. Use the correct option to set the output directory and override the default report name.

Click on `Assessment Report` tab to see the report. If the new changes are not reflected, then reload the report page.

\==============================

```
cd /root/Assessor
sh ./Assessor-CLI.sh -i -rd /var/www/html/ -nts -rp index
```

Benchmarks/Data-Stream Collections: `CIS Ubuntu Linux 20.04 LTS Benchmark v2.0.1`

Profile: `Level 1 - Server`

If the new changes are not reflected on the `Assessment Report` tab, then reload the report page.
{% endhint %}



{% hint style="info" %}

{% endhint %}
