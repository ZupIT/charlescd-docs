---
description: 'Nesta seção, você encontra mais informações sobre a injeção de propriedades.'
---

# Injeção de propriedades

Injeção de propriedades é uma operação importante para preparar suas aplicações nos manifestos realizada pelo Butler.

Veja abaixo quais são:

* **name:**  Nome do recurso Kubernetes.  Alguns recursos manejados pelo Charles precisam ter os seus nomes alterados para que seja possível disponibilizar versões diferentes de uma mesma aplicação em círculos diferentes. A propriedade name terá o seguinte valor: **&lt;originalManifest.metadata.name&gt;-&lt;tag&gt;-&lt;deploymentId&gt;** 
* **originalManifest.metadata.name:** Nome gerado pelo chart da aplicação;
* **tag:** Tag da imagem;
* **deploymentId:** Identificador único da entidade deployment criada pelo Butler.

{% hint style="warning" %}
Essa atualização ocorre apenas nos recursos do tipo **Deployment.**
{% endhint %}

* **namespace:** Namespace destino do deployment. ****Este namespace é especificado durante a configuração do Workspace e indica em qual namespace o deployment acontecerá. Se os charts inserirem este valor nos manifestos, o Charles irá sobrescrevê-los. ****
* **labels:** Labels dos recursos Kubernetes. ****Para que o ciclo de reconciliação do Butler e as rotas criadas pelo Istio funcionem corretamente, é necessário que alguns labels estejam disponíveis em todos os recursos aplicados no cluster. São eles:
  * **deploymentId:** Identificador único da entidade deployment criada pelo Butler;
  * **circleId:** Identificador único do círculo onde o deployment será criado.

### Exemplo

Veja abaixo um exemplo de um manifesto gerado após a compilação do chart:  
****

```text
apiVersion: apps/v1
kind: Deployment
metadata:
  name: http-https-echo
  labels:
    component: http-https-echo
    tag: v1
spec:
  template:
    metadata:
      name: http-https-echo
      labels:
        component: http-https-echo
        tag: v1
    spec:
      containers:
        - name: http-https-echo
          image: mendhak/http-https-echo:latest
  replicas: 1
  selector:
    matchLabels:
      component: http-https-echo

```

Após a injeção de propriedades este mesmo manifesto assumirá a seguinte forma:

```text
apiVersion: apps/v1
kind: Deployment
metadata:
  name: http-https-echo-v1-bc0e1df9-c008-4d86-b534-d782badf3741
  namespace: example-namespace
  labels:
    component: http-https-echo
    tag: v1
    deploymentId: bc0e1df9-c008-4d86-b534-d782badf3741
    circleId: b4b62bc2-4dfd-4673-bc67-cc2cbcf9bb2f
spec:
  template:
    metadata:
      name: http-https-echo
      labels:
        component: http-https-echo
        tag: v1
        deploymentId: bc0e1df9-c008-4d86-b534-d782badf3741
        circleId: b4b62bc2-4dfd-4673-bc67-cc2cbcf9bb2f
    spec:
      containers:
        - name: http-https-echo
          image: mendhak/http-https-echo:latest
  replicas: 1
  selector:
    matchLabels:
      component: http-https-echo

```

  
Depois dessa configuração você pode usar o Charles para realizar o deploy de suas aplicações em círculos segmentados.

