---
description: 'Nesta seção, você encontra como instalar o Charles via Helm.'
---

# Instalando via Helm

{% hint style="info" %}
Antes de prosseguir, tenha certeza de que todos os [**pré-requisitos**](./#pre-requisitos) estão devidamente instalados.
{% endhint %}

Esta instalação é indicada para quem possui uma infraestrutura já montada, devido um ambiente mais complexo ou há algumas limitações de segurança e/ou escalabilidade, o que exige uma **customização mais completa da instalação** do CharlesCD.

### Requisitos 

Para realizar o processo, é necessário ter instalado: 

* \*\*\*\*[**Kubectl**](https://kubernetes.io/docs/tasks/tools/install-kubectl/)\*\*\*\*
* \*\*\*\*[**Helm** ](https://helm.sh/docs/intro/install/)\*\*\*\*

### Como instalar?

Aqui o principal diferencial é a customização. Para isso, foi disponibilizado um **template helm** com todos os campos disponíveis para alteração, incluindo os de banco de dados e recursos consumidos. Você encontra [aqui toda a **documentação dos campos editáveis**.](https://github.com/ZupIT/charlescd/tree/master/install/helm-chart) 

{% hint style="warning" %}
As senhas utilizadas pelo Charles estão armazenadas no arquivo [**values.yaml**](https://github.com/ZupIT/charlescd/blob/master/install/helm-chart/values.yaml)**.** As principais senhas para personalizar estão em:

* butler.database.password
* moove, database.password
* villager.database.password
* circlematcher.redis.password
* keycloak.keycloak.extraEnv.DB\_PASSWORD
* postgresql.postgresqlPassword
* redis.password
* compass.database.password
* hermes.database.password
* rabbitmq.auth.password
* gate.database.password

Para mais detalhes, acesse o link citado acima, que possui toda a documentação dos campos editáveis.
{% endhint %}

Para realizar a instalação com helm charts, basta executar o comando abaixo após a customização dos campos:

```text
// customize tudo que precisa no arquivo values.yaml antes de executar o seguinte comando
helm install charlescd <repo-folder> -n <namespace>
```

{% hint style="warning" %}
É importante lembrar que, caso não seja feita nenhuma customização, o resultado final será igual à instalação via singlefile em que, por padrão, instalamos o PostgreSQL, Redis, Keycloak e Octopipe. 

Por isso, não deixe de customizar os campos caso queira algo gerenciável. 
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

