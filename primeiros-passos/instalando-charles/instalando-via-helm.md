---
description: 'Nesta seção, você encontra como instalar o Charles via Helm.'
---

# Instalando via Helm

{% hint style="warning" %}
Antes de prosseguir, tenha certeza de que todos os [**pré-requisitos**](./#pre-requisitos) estão devidamente instalados.
{% endhint %}

## Como instalar?

O principal diferencial nessa instalação é a customização. E para instalar, você tem acesso a um **template helm** com todos os campos disponíveis para alteração, incluindo os de banco de dados e recursos consumidos. Veja a ****[**documentação dos campos editáveis**](https://github.com/ZupIT/charlescd/tree/master/install/helm-chart)**.** 

{% hint style="info" %}
As senhas utilizadas pelo Charles estão armazenadas no arquivo [**values.yaml**](https://github.com/ZupIT/charlescd/blob/main/install/helm-chart/values.yaml)**.** As principais senhas para personalizar são:

* CharlesApplications.butler.database.password
* CharlesApplications.moove.database.password
* CharlesApplications.villager.database.password
* CharlesApplications.circlematcher.redis.password
* CharlesApplications.keycloak.keycloak.extraEnv.DB\_PASSWORD
* CharlesApplications.postgresql.postgresqlPassword
* CharlesApplications.redis.password
* CharlesApplications.compass.database.password
* CharlesApplications.hermes.database.password
* CharlesApplications.rabbitmq.auth.password
* CharlesApplications.gate.database.password
* CharlesApplications.compass.moove.database.password

Para mais detalhes, acesse o link citado acima, que possui toda a documentação dos campos editáveis.
{% endhint %}

Para instalar com helm charts, execute o comando abaixo, dentro da pasta _**/charlescd/install/helm-chart,**_ após a customização dos campos: 

```text
helm install --create-namespace -n <namespace> charlescd . -f values.yaml
```

{% hint style="warning" %}
É importante lembrar que, caso não seja feita nenhuma customização, por padrão o Charles instala o PostgreSQL, Redis, Keycloak e RabbitMQ.  Por isso, não deixe de customizar os campos caso queira algo gerenciável. 
{% endhint %}

### Troque a senha padrão \(default password\) 

Depois de instalar o CharlesCD, troque algumas **senhas padrão**, veja abaixo: 

**Senha do Keycloak**:   
**1.** Acesse: **http://&lt;charlescd-url&gt;/keycloak/auth;**  
**2.** Clique em **Administration Console;**   
**3.**  Insira a senha do usuário do Keycloak \(admin - firstpassword\) e troque a senha padrão.

**Senha do CharlesCD:**  
Acesse o CharlesCD e logue com  
**1. Usuário:** **charlesadmin@admin  
2. Senha**: **charlesadmin;  
3.** Vá até **Account &gt; Profile** e depois em **Change Password** e escolha sua nova senha.

