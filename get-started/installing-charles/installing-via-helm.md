---
description: 'In this section, you will find how to install Charles with Helm.'
---

# Installing via Helm

{% hint style="warning" %}
Before proceeding, make sure all the [**requirements**](./#requirements) are properly installed.
{% endhint %}

## How to install? 

This installation is customizable, to install access a **helm template** with all the available fields to change, including the database and the consumed resources. Check out the [**documentation of editable fields**](https://github.com/ZupIT/charlescd/tree/main/install/helm-chart).

{% hint style="info" %}
The passwords used by Charles are stored in the [**values.yaml**](https://github.com/ZupIT/charlescd/blob/main/install/helm-chart/values.yaml) ****file.  The main passwords to customized are:

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

For more details, [**access editable fields documentation**](https://github.com/ZupIT/charlescd/tree/main/install/helm-chart).  
{% endhint %}

* To make sure the charts dependencies are present and updated with a compatible version, use in the **/charlescd/install/helm-chart** folder the command below: 

```text
helm install --create-namespace -n <namespace> charlescd . -f values.yaml
```

* To install with Helm Charts,  after you have customized the fields, run the command below inside the _**/charlescd/install/helm-chart**_ folder: 

```text
helm install --create-namespace -n <namespace> charlescd . -f values.yaml
```

{% hint style="warning" %}
If you don't customize anything, Charles installs by default **PostgreSQL**, **Redis**, **Keycloak**, and **RabbitMQ.**  So, don't forget to customize the fields if you want something manageable. 
{% endhint %}

### Change the default passwords

After installing CharlesCD, remember to change some **default passwords,** check out below:

**Keycloak password**:   
**1**. Access: **http://&lt;charlescd-url&gt;/keycloak/auth;**  
**2**. Click on **Administration Console;**   
**3.** Enter with Keycloak user and password \(admin - firstpassword\) and change the default password.  


**CharlesCD password:**   
Log in CharlesCD with:  
**1**. **User:** charlesadmin@admin  
**2. Password:** charlesadmin;  
**3.** Go to A**ccount &gt; Profile** and then **Change Password.**

