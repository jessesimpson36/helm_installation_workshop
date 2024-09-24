
# 1_typical_helm_install


## How to run

```bash
helm install -f values.yaml test ./chart
```

### What next?

Keep note of what happens to the output of the following commands:


Get the installer-supplied `values.yaml`
```bash
helm get values test
```

Output
```yaml
elasticsearch:
  replicas: 2
```

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

Notice replicas 2 is used instead of 1. This isn't very remarkable, but both commands will be important in the next section.

## uninstall

```bash
helm uninstall ./chart
```

