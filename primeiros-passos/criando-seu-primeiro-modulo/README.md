---
description: Esta seção descreve como você pode criar seu primeiro módulo no Charles.
---

# Criando seu primeiro módulo

Depois de criar e configurar o seu workspace, agora é necessário adicionar os módulos. 

{% hint style="info" %}
Um **módulo** é a sua aplicação que está armazenada em um repositório do ****[**Git cadastrado anteriormente**.]()
{% endhint %}

### Como adicionar os módulos? 

Para adicionar os módulos, acesse o menu **Modules** __no **workspace** desejado, e __siga os seguintes passos:

* **Nome**: o nome deverá ser a junção da organização e a do módulo, como está no git. Por exemplo: `ZupIt/charlescd`.
* **Git URL**: informe a URL do seu repositório. Por exemplo: [https://github.com](https://github.com/ZupIT/charlescd).
* **Helm repository URL:** informe o repositório onde o está o template do helm da sua aplicação.

{% hint style="info" %}
Para mais informações sobre a configuração do repositório do helm, [**acesse aqui.**](configurando-o-chart-template.md)\*\*\*\*
{% endhint %}

Se seu repositório tem várias aplicações, cadastre-as como componentes e informe: 

* **Nome**: o nome da aplicação, conforme está no repositório.
* **Métricas**: latência \(ms\) e erro HTTP \(%\). Em ambos os casos, deve-se informar um valor de risco que você gostaria de ser alertado caso seu componente alcançasse ou ultrapassasse.

![](../../.gitbook/assets/image%20%288%29.png)

## O que são componentes?

Componentes são abstrações das aplicações. Se dentro do seu repositório há múltiplas aplicações, cada componente corresponderá a uma delas.

### Métricas de saúde

Para cada componente é possível cadastrar as seguintes métricas para análise de saúde: **latência** \(ms\) e **erro HTTP** \(%\). Quando os limites forem atingidos, ou estiverem à menos de 10%, você receberá um alerta informando o estado da sua aplicação no círculo que demonstra o problema.

