
# 3_using_subchart

In this scenario, I will create a new helm chart called jesse-prod-env, to represent all  helm charts that I want to install on my cluster. For now, this will only include 1 subchart, but in the future, I may want a postgresql database, a mongodb instance, and even an IBM DB2 instance.

## How to run

```bash
helm install -f values.yaml test ./chart
```

### What next?

In this helm installation, I have a new helm chart called `jesse-prod-env`, with a Chart.yaml inside the `chart` folder. This helm chart will install a subchart.  What's unique about this type of installation is that in our values.yaml, we now have a new section:

```
nil_bool:
```

This was actually the name of the helm chart I used in previous examples. all existing chart default `values.yaml` options are available under the `nil_bool` key. However, the global section is not underneath the nil_bool key because global is a special key shared between all subcharts.


## uninstall

```bash
helm uninstall ./chart
```

