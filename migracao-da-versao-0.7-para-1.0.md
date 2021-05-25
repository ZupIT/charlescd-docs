---
description: >-
  Nesta seção, você vai encontrar como realizar a migração do Charles da versão
  0.7.1 para 1.0.0.
---

# Migração da versão 0.7.1 para 1.0.0

A nova versão 1.0.0 do CharlesCD trouxe mais confiabilidade nos deploys com o [**Operator**](referencia/preparando-seu-deploy.md), porém o Helm encontra dificuldades para adicionar elementos necessários ao Operator em uma instalação já existente, como o **Custom Resources Definition** \(CRDs\). Por isso, você precisa seguir alguns passos para uma atualização mais funcional.  Veja abaixo: 

### **Passo 1: Desinstale o CharlesCD \(instalação feita com o Helm\):**

{% hint style="warning" %}
Este passo **remove tudo**, inclusive o banco de dados. Se seu banco de dados é **externo** ou se **está dentro do Kubernetes,** seus dados não serão apagados, porque os volumes persistem mesmo após uma desinstalação via Helm. 
{% endhint %}

Para desinstalar, execute o seguinte comando:

```text
helm uninstall -n <NAMESPACE_CHARLESCD> <NOME_DA_INSTALAÇÃO>
```

### **Passo 2: Instale o CharlesCD utilizando o Helm:**

Agora instale o CharlesCD via _Helm._ Utilize o seu arquivo de _Values_ da instalação original validando para cada manifesto se houve a adição de novos valores. 

{% hint style="info" %}
Você precisa validar fazendo um diff do seu arquivo de instalação e do disponível no repositório do CharlesCD.
{% endhint %}

Para instalar, execute o comando abaixo:

```text

helm install -n <NAMESPACE_CHARLESCD> <NOME_DA_INSTALAÇÃO> ./install/helm-chart/ -f ./install/helm-chart/meu-values.yaml

```

### **Passo 3: Atualize a configuração de Deployment  para cada Workspace:**

Depois de instalar, você poderá acessar o CharlesCD com seu usuário administrador da plataforma**.** É necessário fazer uma nova configuração de deployment para seus workspaces.  Veja como fazer essa configuração em [**Configuração de Deployment**](referencia/preparando-seu-deploy.md#configuracao-de-deployment)**.** 

###  **Passo 4: Faça o override de release em cada círculo ativo**

Agora, você precisa fazer o Override de Release com a última release em que o deployment em cada círculo ativo foi feito. Veja como realizar overrides de versões em '[**Como criar releases pelo Charles?**](referencia/release.md#como-criar-releases-pelo-charles)'.

{% hint style="info" %}
Não se preocupe com o override de release. O CharlesCD irá reconhecer todos os manifestos que ele precisa tomar conta.
{% endhint %}

### **Passo 5: Remova todos os manifestos em que o CharlesCD fez os deployments antes da versão 1.0.0:**

Você precisa remover todos os manifestos em que o CharlesCD fez os deployments antes da versão 1.0.0. Ao deletar esse esses manifestos antigos, o Operator pode recriá-los e com isso, a sua aplicação vai funcionar em menos de 30 segundos. A indisponibilidade é em média de 5 segundos.

Siga os passos para remoção: 

1. Você precisa saber ****todos os manifestos existentes no namespace do seu workspace que podem ser deletados. Para isso execute o seguinte comando, por worspace:

```text
kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found  -n <NAMESPACE_WORKSPACE> -o go-template --template '{{range .items}}{{.kind}}/{{.metadata.name}} {{.metadata.creationTimestamp}}{{"\n"}}{{end}}' 2>&1 | grep -i -v "Warn" | grep -i -v "Deprecat" | awk '$2 <= "'$(date -d '1 hour ago' -Ins --utc | sed 's/+0000/Z/')'" { print $1 }' | grep -i -v "ConfigMap/istio" | grep -i -v "ConfigMap/kube" | grep -i -v "Secret/default" | grep -i -v "ServiceAccount/default"

```

Este comando mostra uma lista de manifestos relacionados ao seu workspace mais antigo que 1 hora, isto é, tudo que não houve o deployment no namespace de seu Kubernetes. Veja abaixo um exemplo:

![](.gitbook/assets/image%20%2822%29.png)

2. Se não existir manifestos  aplicados por fora do CharlesCD, um **nginx** para servir de ingress para sua aplicação, por exemplo. Adicione no comando:

```text
 | grep -i -v "PALAVRA_CHAVE"
```

Veja como é exemplo do nginx: 

```text
 
kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found  -n charlesapp -o go-template --template '{{range .items}}{{.kind}}/{{.metadata.name}} {{.metadata.creationTimestamp}}{{"\n"}}{{end}}' 2>&1 | grep -i -v "Warn" | grep -i -v "Deprecat" | awk '$2 <= "'$(date -d '1 hour ago' -Ins --utc | sed 's/+0000/Z/')'" { print $1 }' | grep -i -v "ConfigMap/istio" | grep -i -v "ConfigMap/kube" | grep -i -v "Secret/default" | grep -i -v "ServiceAccount/default" | grep -i -v "nginx"

```

Esse comando remove da listagem o manifesto e depois você terá a lista completa para remoção do cluster.

3. Quando você estiver com essa lista de manifestos, delete os manifestos e o restante será executado pelo Operator. Execute o comando abaixo:

```text
kubectl delete -n <namespace> <manifesto>
```

{% hint style="success" %}
Pronto! O processo de atualização foi finalizado :\) 
{% endhint %}

