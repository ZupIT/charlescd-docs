# Azure Container Registry

Informe os dados abaixo para conceder acesso a Charles ao seu Azure Container Registry:

* **Nome**: esse nome representará sua configuração no Charles;
* **URL do seu registry**: a URL para o seu registry padrão é [https://**registry-name**.azurecr.io/](https://registry-name.azurecr.io/)
* **Username**: ID da entidade de serviço que será usada pelo Kubernetes para acessar o registro;
* **Password**: senha da entidade de serviço.

Em caso de dúvidas para encontrar essas informações, sugerimos a documentação da [**Azure Container Registry**](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-concepts).

{% hint style="info" %}
A funcionalidade de Connection Test garante que as credenciais informadas são válidas.
{% endhint %}

