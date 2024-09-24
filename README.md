# helm_installation_workshop
Explaining the differences between different ways of installing a helm chart.


## 1_typical_helm_install

Typical helm installations have a `values.yaml` that the the installer uses alongside the helm chart itself. A helm chart has it's own `values.yaml` which is combined with the installers `values.yaml` to make the overall set of options that creates the kubernetes manifests with.

There is a distinction between the installer's `values.yaml` and the default `values.yaml` provided with a helm chart. In this repo, I will always state which one I'm referring to.

A typical helm install looks like this:

```bash
helm install -f values.yaml release repo/chart
```
and there is a variation of this where you can specify a tgz of the chart (output of `helm package`).

```bash
helm install -f values.yaml release chart.tgz
```

Those two mechanisms are functionally equivalent.

## 2_forked_helm_install

In this helm installation, the installer will take a chart and modify the default `values.yaml` provided by the helm chart.



## 3_using_subchart

In this form of installation, the customer will create a new helm chart that uses your subchart as a dependency. An installation this way has a unique characteristic that the `values.yaml` will often have options underneath a sub-option.




