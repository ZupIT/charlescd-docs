---
description: >-
  Nesta seção você encontra detalhes de como configurar o módulo de deploy do
  Charles e como preparar suas aplicações para serem disponibilizadas no
  cluster.
---

# Preparando o seu Deploy

O módulo de deploy aplica e monitora os recursos no cluster. E para fazer isso, ele utiliza o padrão [**Operator**](https://kubernetes.io/docs/concepts/extend-kubernetes/operator) ****que realiza os ciclos de reconciliação garantindo que o cluster sempre estará no estado que você precisa. Os logs do Kubernetes também serão coletados em tempo real.

## **Configuração de Deployment**

O Charles possui uma arquitetura adaptável a diferentes instalações Kubernetes. O único requisito é que seu módulo de deploy esteja instalado no cluster de destino com uma URL acessível. A configuração de deployment indica qual é essa URL e quais credenciais do Git serão utilizadas para buscar os charts helm. Sem esta configuração o Charles não conseguirá realizar deploys.

### **Como cadastrar a configuração?**

Siga os passos abaixo:

1. Em **Workspaces,** no menu lateral esquerdo, selecione o workspace desejado, em seguida selecione **Settings ;**
2. Clique em **Credentials;**
3. Clique em **Add Deployment Configuration;**

Preencha os seguintes campos:

1. **Butler URL:** URL do módulo de deploy Butler. Caso este esteja no mesmo cluster de instalação do Charles, utilize seu FQDN \(Fully Qualified Domain Name\). Exemplo: http://charlescd-butler.butler-namespace.svc.cluster.local:3000.
2. **Namespace:** Defina o namespace em que os recursos serão disponibilizados no cluster. Você deve criar o seu namespace, uma vez que o Charles não faz isso;
3. **Git provider:** Defina o provedor de git a ser utilizado \(**GitHub** ou **GitLab**\);
4. **Git token:** Insira um token de autenticação que tenha acesso repositório git onde está armazenado seus [**templates Helm**](../primeiros-passos/criando-seu-primeiro-modulo/configurando-o-chart-template.md) que serão utilizados durante o deployment da sua [**aplicação**](../primeiros-passos/criando-seu-primeiro-modulo/). Caso o seu Git Provider seja **GitHub**, é necessário a permissão "_repo_". Se não, configure no **GitLab** os acessos: "_api_ " e "_read\_repository_". 

## **Configurando sua aplicação**

O módulo de deploy Butler utiliza charts helm para disponibilizar as suas aplicações no Cluster. Esses charts devem estar disponíveis em um repositório Github ou Gitlab e acessíveis por meio do token cadastrado na configuração de deployment. A URL deles é providenciada junto ao cadastro do módulo.

### **Charts**

Os charts devem seguir o [**padrão do Helm**](https://helm.sh/docs/topics/charts/), e precisam ****estar contidos dentro de uma pasta com o nome da componente cadastrada no Charles. Você não precisa executar nenhum comando para empacotar o chart, o Charles faz o download dos arquivos e finaliza tudo automaticamente.  
  
Veja abaixo o exemplo de um repositório contendo o chart da componente **http-https-echo** no GitHub:

![](https://lh5.googleusercontent.com/Rt7_Lw1DbK152QKt3brsCYyzF0DAQ4wuoWsdCVyUaZjf9Hlh64EaK7YnHjF16W_xo2BQzlUJyUeUsooPzqwmMIKF7ttUXRej3eM56uWu6WH4QNCiByixeV4zEdHLwEGRq7NCruhH)

### **Templates**

Os templates também não precisam de nenhum tratamento especial. O único requisito é que gerem manifestos válidos.  
****  
O Butler armazena internamente os charts compilados em entidades que representam cada solicitação de deploy. Com isso, você pode realizar rollbacks mais eficientes. 

### **Injeção de propriedades**

Uma operação muito importante e que deve ser levada em consideração no momento de preparar suas aplicações é a injeção de propriedades nos manifestos realizada pelo Butler. São essas:

* **name:**  Nome do recurso Kubernetes.  Alguns recursos manejados pelo Charles precisam ter os seus nomes alterados para que seja possível disponibilizar versões diferentes de uma mesma aplicação em círculos diferentes. A propriedade name terá o seguinte valor: **&lt;originalManifest.metadata.name&gt;-&lt;tag&gt;-&lt;deploymentId&gt;** 
* **originalManifest.metadata.name:** Nome gerado pelo chart da aplicação;
* **tag:** Tag da imagem;
* **deploymentId:** Identificador único da entidade deployment criada pelo Butler.

{% hint style="warning" %}
Essa atualização ocorre apenas nos recursos do tipo **Deployment.**
{% endhint %}

* **namespace:** Namespace destino do deployment. ****Este namespace é especificado durante a configuração do Workspace e indica em qual namespace o deployment acontecerá. Se os charts inserirem este valor nos manifestos, o Charles irá sobrescrevê-los. ****
* **labels:** Labels dos recursos Kubernetes. ****Para que o ciclo de reconciliação do Butler e as rotas criadas pelo Istio funcionem corretamente, é necessário que alguns labels estejam disponíveis em todos os recursos aplicados no cluster. São eles:
  * **deploymentId:** Identificador único da entidade deployment criada pelo Butler;
  * **circleId:** Identificador único do círculo onde o deployment será criado.

Depois dessa configuração você pode usar o Charles para realizar o deploy de suas aplicações em círculos segmentados.

Veja um exemplo abaixo de um manifesto gerado após a compilação do chart:  
****

![](https://lh3.googleusercontent.com/IASfFV_yrgYkA2mqTDsmV8eDO9gLX2AhZQYRNO7_s_vE-uVAm8buuAtGMXKD3n2QCxlf8CIAYFflkd9dYQabUimnc0F01IE27hqltR5pPeOkGuHBOwuUnOeBQ5PAHXbi-ivrdTMy)

  
****Após a injeção de propriedades este mesmo manifesto assumirá a seguinte forma:

![](https://lh6.googleusercontent.com/CcDZyXf051HHlMSkMdT9SB6G5CNM8bhKwQx19nu9zam0uJCPs69flh5QIkKq3iri4bmO_QLvePJ0KVupU3N3THEOtpa7hWJU6nkaOaSuKKgpGchEcGausbtGH0YCGBR_zeu4x4Vk)

  


