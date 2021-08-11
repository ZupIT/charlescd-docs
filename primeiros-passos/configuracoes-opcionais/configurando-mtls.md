---
description: 'Nesta seção, você encontra como configurar o MTLS na sua instalação.'
---

# Configurando MTLS

{% hint style="info" %}
A opção de _Mutual Transport Layer Security_ \(MTLS\) é recomendada quando você quiser instalar o Charles em **um cluster diferente de onde estão suas aplicações.**
{% endhint %}

Essa configuração é necessária para garantir uma comunicação segura entre o serviços no Charles, já que  podem estar expostos em cluster diferentes. Nesse cenário, apenas um componente do Charles precisa estar no mesmo cluster que as suas aplicações, o `charlescd-butler`.

## Como habilitar o MTLS? 

 Siga os passos abaixo para habilitar o MTLS na sua aplicação: 

**Passo 1:**  Acesse **`charlescd/install/mtls-job/values.yaml`**  e coloque o domínio em que o Charles será utilizado , veja o exemplo abaixo: 

```text
mtls:
  domain: .charles.com
  enabled: true
```

**Passo 2:** Execute os manifestos, eles geram as secrets com os certificados de cada componente e a autoridade certificadora.  

{% hint style="warning" %}
Os certificados gerados são auto-assinados.
{% endhint %}

**Passo 3:** Na pasta raiz do projeto, execute o comando:

```yaml
helm install mtls-job ./install/mtls-job/ --namespace charlescd --values=./install/mtls-job/values.yaml
```

**Passo 4:** Se você quiser utilizar certificados próprios e não aqueles gerados pelo job, utilize o ****[**script de 'create secrets'**](https://github.com/ZupIT/charlescd/blob/security/mtls/install/helm-chart/scripts/create-tls-secrets.sh) para auxiliar na criação das secrets.

**Passo 5:** Copie as secrets de um cluster para outro. E depois:

* Copie as secrets  dentro de manifestos yaml, para fazer isso utilize o [**script de 'copy secrets'.**](https://github.com/ZupIT/charlescd/blob/security/mtls/install/helm-chart/scripts/copy-secrets.sh)\*\*\*\*
* Aplique os manifestos no cluster destino, use o ****[**script de 'apply secrets'**](https://github.com/ZupIT/charlescd/blob/security/mtls/install/helm-chart/scripts/apply-secrets.sh)**.**

**Passo 6:** Acesse **`charlescd/install/helm-chart/values.yaml`** nas configurações do moove e mude o valor da propriedade  **`mtls/enabled`** para **`true` .** Veja abaixo: ****

```yaml
moove:
    mtls:
      enabled: true
```

**Passo 7:** Faça a mesma alteração do passo 6, mas agora para o **componente butler**, quando você instalá-lo em outro cluster, veja o exemplo: 

```yaml
butler:
    mtls:
        enabled: true
```

**Passo 8:** Depois disso, siga os passos da instalação do Charles.

