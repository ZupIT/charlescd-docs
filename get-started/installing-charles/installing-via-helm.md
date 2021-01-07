# Installing via Helm

{% hint style="info" %}
Before proceeding, make sure that all the [**requirements**](./#requirements) are properly installed.
{% endhint %}

This installation is recommended for those who already has an infrastructure to deal with a more complex environment or who has some limitations of security/scalability, which demands a **more complete install customization** of CharlesCD.  

### Requisites 

To run the process, you must have the following programs:

* \*\*\*\*[**Kubectl**](https://kubernetes.io/docs/tasks/tools/install-kubectl/)\*\*\*\*
* [**Helm** ](https://helm.sh/docs/intro/install/)\*\*\*\*

### How does it works?

This installation is recommended if you want a specific customization. To make this happen, there is a helm template with all the available fields to be altered, including the database and consumed resources. You will find the documentation with the[ **editable fields here**](https://github.com/ZupIT/charlescd/tree/master/install/helm-chart).

{% hint style="warning" %}
The passwords used by Charles are stored in the [**values.yaml**](https://github.com/ZupIT/charlescd/blob/master/install/helm-chart/values.yaml) ****file.  The main passwords to customized are:

* butler.database.password
* moove, database.password
* villager.database.password
* circlematcher.redis.password
* keycloak.keycloak.extraEnv.DB\_PASSWORD
* postgresql.postgresqlPassword
* redis.password
* compass.database.password

For more details, access the link mentioned before about editable fields. 
{% endhint %}

To complete the installation with helm charts, just run the command below after you customized the fields: 

```text
// customize everything you need in the file values.yaml before you execute the following command
helm install charlescd <repo-folder> -n <namespace>
```

{% hint style="warning" %}
It's important to remember that, in case of no customization at all, the final result is the same as in case \#1 in which, for standard, we install the PostgreSQL, Redis, Keycloak and CharlesCD. 

So, you must not forget to customize the fields in case you want something manageable. 
{% endhint %}

