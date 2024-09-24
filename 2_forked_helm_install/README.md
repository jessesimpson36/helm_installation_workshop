
# 2_forked_helm_install


## How to run the first chart?

```bash
helm install test ./chart
```

### What happened to values.yaml ?

In this installation type, the installer chooses to overwrite the defaults from the chart they copied. As an installer, it's important to keep all options there, unless you are explicitly overriding them with something else.


In the `chart` folder, I modified the default to set replicas=2 instead of the previous `1_typical_helm_install`'s replicas=1.  This is mostly fine because you're setting an option to what you think should be the default.

Get the installer-supplied `values.yaml`
```bash
helm get values test
```

Output
```yaml
null
```
There is no output here because you don't specify a installer's `values.yaml` (you can if you wanted to override the new defaults.)

Get the combination of the charts' default values.yaml options with overwrites by the users `values.yaml`.
```bash
helm get values -a test
```

Output:
```yaml
elasticsearch:
  replicas: 2
global:
  elasticsearch:
    tls:
      existingSecret: null
  opensearch:
    tls:
      existingSecret: null
```


## uninstall the chart

```bash
helm uninstall ./chart
```
## Installing the bugged chart

In this chart, I modified the charts' default `values.yaml`. I  replaced the previous section:

```yaml
global:
  opensearch:
    tls:
      existingSecret:
  elasticsearch:
    tls:
      existingSecret:
``` 

with this version which will drop the tls.existingSecret and instead add enabled: false

```yaml
global:
  opensearch:
    tls:
      existingSecret:
  elasticsearch:
    enabled: false
``` 


```bash
helm install test ./chart_bugged
```

output:
```
Error: INSTALLATION FAILED: template: nil_bool/templates/statefulset.yaml:24:29: executing "nil_bool/templates/statefulset.yaml" at <.Values.global.elasticsearch.tls.existingSecret>: nil pointer evaluating interface {}.existingSecret
```


We get this error because the `statefulset.yaml` template in the templates folder is checking for `.Values.global.elasticsearch.tls.existingSecret`, an option that no longer exists because the installer removed it.



## What if we ran the values.yaml from the bugged chart as though it were a typical helm install?

In the chart_fixed folder, I restored the charts default `values.yaml` that were used in the first example, and when we install this helm chart, we will then supply the values.yaml from the bugged example.

### How to run chart_fixed

```bash
helm install -f chart_bugged/values.yaml test chart_fixed
```

Output:

```
NAME: test
LAST DEPLOYED: Tue Sep 24 12:06:35 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```
This works because the helm chart knows that a variable named `global.elasticsearch.tls.existingSecret` exists. Because the charts defaults `values.yaml` includes this option.

```bash
helm get values test
```

```yaml
elasticsearch:
  replicas: 1
global:
  elasticsearch:
    enabled: false
  opensearch:
    tls:
      existingSecret: null
```

```bash
helm get values -a test
```

Output:
```yaml
elasticsearch:
  replicas: 1
global:
  elasticsearch:
    enabled: false
    tls:
      existingSecret: null
  opensearch:
    tls: {}
```


