---
description: Esta seção descreve como configurar o chart template no ambiente do Charles.
---

# Configurando o chart template

## **O que é o Helm?**

O Helm Charts é um gerenciador de pacotes que permite você definir, instalar e atualizar as aplicações no Kubernetes, independente do grau de complexidade.  

No contexto do Charles, o [**Chart Template**](https://helm.sh/docs/chart_template_guide/getting_started/) ****é usado como uma coleção de arquivos relacionados a configurações do Kubernetes. 

{% hint style="info" %}
Se você não tiver configurado o **seu módulo,** [**acesse aqui**](./). É importante lembrar que você deve cadastrar a URL no módulo.
{% endhint %}

## Como configurar o chart template? 

Siga os próximos passos para configurar nosso app de exemplo.

### **Passo 1: Crie um diretório do chart template**

Para começar, você precisa salvar os seus templates em uma ferramenta de versionamento da sua preferência. Assim que criar um novo chart template, você precisa dar ao diretório o mesmo nome do componente ao qual ele se refere**.** 

 A estrutura abaixo contém os templates necessários para se fazer o deploy de um módulo que possui um componente chamado “circles-sample”. 

A imagem demonstra como seu diretório deve ficar:  

![ Diret&#xF3;rio de chart template do circle-sample](../../.gitbook/assets/screen-shot-2020-07-24-at-16.17.05.png)

### Passo 2: Configure os itens do diretório 

Depois de criado o diretório, você deve configurá-lo. Veja quais arquivos são necessários para seguir com essa configuração: 

* **templates/ :** contém nossos modelos. 

  * **deployment.yaml:** descreve a estrutura de [**deployment**](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/).
  * **service.yaml:** descreve a estrutura do [**service**](https://kubernetes.io/docs/concepts/services-networking/service/). 

* O arquivo **Chart.yaml** contém uma descrições como version, name, description. É necessário definir a version como "darwin". 
* O arquivo **value.yaml** possui os valores que serão utilizados nos nossos templates. 

Essas são as informações que o Charles precisa ter no templates. Vale ressaltar que você pode incrementar esses templates da forma como você preferir.

{% hint style="info" %}
Com o seu diretório configurado de acordo com a estrutura acima, vá até a pasta "circles-sample" e execute o comando  **"`helm package .`"**.  

Ao final desse comando, você terá uma arquivo **tgz** com o nome de circles-samples-darwin. Nossa ferramenta de CD procura esse **tgz** para executar o template
{% endhint %}

