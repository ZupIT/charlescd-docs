# Docker Registry

Durante a configuração do seu workspace, é necessário cadastrar o registry onde as imagens das suas aplicações estão armazenadas. Esse acesso é importante, pois uma vez configurado, o CharlesCD pode **observar novas imagens sendo geradas e listar as imagens já salvas no seu registry** para implantá-las.

Existem duas categorias de cadastros de configuração que podem ser feitas pelo workspace: _AWS_ e _Azure._ Você escolhe qual delas deseja seguir e adiciona as seguintes informações:

### AWS

* **Nome**: esse nome representará sua configuração no Charles;
* **URL do seu registry**: segundo a convenção, a URL para o seu registry padrão é  [https://**aws\_account\_id**.dkr.ecr.region.amazonaws.com](https://aws_account_id.dkr.ecr.region.amazonaws.com);
* **Access Key**: informação de segurança gerada pela AWS ECR;
* **Secret Key**: informação de segurança gerada pela AWS ECR;
* **Region**: a região de onde você está operando. 

Em caso de dúvidas para encontrar essas informações, sugerimos a documentação da [**Amazon ECR**](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html).

### AZURE

* **Nome**: esse nome representará sua configuração no Charles;
* **URL do seu registry**: a URL para o seu registry padrão é  **https://{registry name}.**[**azurecr.io**](http://azurecr.io/)\*\*\*\*
* **Username**: ID da entidade de serviço que será usada pelo Kubernetes para acessar o registro;
* **Password**: Senha da entidade de serviço.

Em caso de dúvidas para encontrar essas informações, sugerimos a documentação da [**Azure Container Registry**](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-concepts).

### Google Cloud Platform - GCP

* **Nome**: esse nome representará sua configuração no Charles;
* **URL do seu registry**: a URL do seu GCR, como[ ](%20https://gcr.io)\*\*\*\*[**https://gcr.io**](%20https://gcr.io);
* **Project ID:** acesse o _Google Cloud Platform_, selecione seu projeto e ****[**copie o id**;](https://support.google.com/googleapi/answer/7014113?hl=en)
* **JSON key:** adicione o JSON key gerado. ****Para mais informações de [**como gerar seu json key, veja aqui**](https://cloud.google.com/container-registry/docs/advanced-authentication#json-key)**.**

Em caso de dúvidas para encontrar essas informações, sugerimos a documentação da [**Google Container Registry**](https://cloud.google.com/container-registry).  


### Docker Hub 

* **Nome**: esse nome representará sua configuração no Charles;
* **URL do seu registry**: a URL do seu DockerHub [**https://registry.hub.docker.com**](https://registry.hub.docker.com/)**;** 
* **Username:**  adicione o seu **dockerid**;
* **Password:** a senha do seu dockerhub ou o **token de acesso**. Veja a baixo como gerar: 

1. Acesse sua conta docker;
2. Vá em configurações \(**Account settings**\);
3. Selecione a opção **Security**;
4. Clique em **new access token**  e copie o token gerado. 





