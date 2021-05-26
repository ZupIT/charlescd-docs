---
description: >-
  Nessa seção, você encontra como realizar a autenticação com seu registry
  privado.
---

# Autenticando com registry privado

## Por que autenticar?

Em casos de registries privados, essa autenticação garantirá o sucesso na comunicação com o seu [**registry**](../definindo-workspace/docker-registry.md) quando for preciso fazer o pull das imagens durante alguma implantação.

## Como autenticar?

O cluster Kubernetes utiliza o tipo **secret** do **docker-registy** para autenticar o registry container. 

{% hint style="info" %}
 Para mais informações de como gerar esse **secret** para ser ****aplicado no seu cluster, [**acesse a documentação do Kubernetes**](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/). 
{% endhint %}

Uma vez gerado o secret, ele ter uma formatação parecida com o exemplo abaixo:

```text
apiVersion: v1
data:
  .dockerconfigjson: <<your value>>
kind: Secret
metadata:
  name: <<registry-name>>
type: kubernetes.io/dockerconfigjson
```

Com o secret em mãos, você precisará aplicar essa informação no namespace onde suas aplicações serão implantados pelo Charles:

```text
kubectl -n your-namespace apply secret-registry.yaml
```

Finalizando desses passos, seu cluster conseguirá manter a comunicação com o registry.

