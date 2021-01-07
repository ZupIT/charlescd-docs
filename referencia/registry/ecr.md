# ECR

Informe os dados abaixo para conceder acesso a Charles ao seu Amazon ECR:

**Nome**: esse nome representará sua configuração no Charles;

* **URL do seu registry**: segundo a convenção, a URL para o seu registry padrão é  [https://**aws\_account\_id**.dkr.ecr.region.amazonaws.com](https://aws_account_id.dkr.ecr.region.amazonaws.com);
* **Access Key**: informação de segurança gerada pela AWS ECR;
* **Secret Key**: informação de segurança gerada pela AWS ECR;
* **Region**: a região de onde você está operando. 

Em caso de dúvidas para encontrar essas informações, sugerimos a documentação da [**Amazon ECR**](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html).

{% hint style="info" %}
A funcionalidade de Connection Test garante que as credenciais informadas são válidas.
{% endhint %}

