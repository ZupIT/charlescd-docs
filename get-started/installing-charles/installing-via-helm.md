---
description: 'In this section, you will find how to install Charles with Helm.'
---

# Installing via Helm

{% hint style="info" %}
Before proceeding, make sure that all the [**requirements**](./#requirements) are properly installed.
{% endhint %}

This installation is recommended for those who already have an infrastructure to deal with a more complex environment or who have some limitations of security/scalability, which demands **more complete install customization** of CharlesCD.  

### Requisites 

To run the process, you must have the following programs:

* \*\*\*\*[**Kubectl**](https://kubernetes.io/docs/tasks/tools/install-kubectl/)\*\*\*\*
* [**Helm** ](https://helm.sh/docs/intro/install/)\*\*\*\*

### How does it work?

This installation is recommended if you want specific customization. To make this happen, there is a helm template with all the available fields to be altered, including the database and consumed resources. You will find the documentation with the[ **editable fields here**](https://github.com/ZupIT/charlescd/tree/master/install/helm-chart).

{% hint style="warning" %}
The passwords used by Charles are stored in the [**values.yaml**](https://github.com/ZupIT/charlescd/blob/main/install/helm-chart/values.yaml) ****file.  The main passwords to customized are:

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

For more details, access the link mentioned before about editable fields. 
{% endhint %}

To complete the installation with helm charts, just run the command below after you customized the fields: 

```text
// customize everything you need in the file values.yaml before you execute the following command
helm install charlescd <repo-folder> -n <namespace>
```

{% hint style="warning" %}
It's important to remember that, in case of no customization at all, the final result is the same as in case \#1 in which, for standard, we install the PostgreSQL, Redis, Keycloak, and CharlesCD. 

So, you must not forget to customize the fields in case you want something manageable. 
{% endhint %}



### Change default passwords

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

