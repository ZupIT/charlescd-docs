# Configuring your ingress

If you want to use your own ingress instead of the one provided with Charles' installation, follow the next step: 

* On`charlescd/install/helm-chart/values.yaml`, change the **`enabled`** value to **`false`**, like the example below:

```text
host: charles.info.example
  class: nginx
  controller:
  nginx:
  enabled: false
```

