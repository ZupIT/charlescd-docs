---
description: 'Nesta seção, você encontra sobre como configurar a sua ingress.'
---

# Configurando sua ingress

Caso você queira utilizar sua ingress, ao invés da que possui na instalação do Charles, siga o seguinte passo:

* em `charlescd/install/helm-chart/values.yaml`, nas configurações de ingress, mude o valor de **`enabled`** para **`false`** como no exemplo abaixo.

```text
host: charles.info.example
  class: nginx
  controller:
  nginx:
  enabled: false
```
