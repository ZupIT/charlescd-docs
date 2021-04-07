---
description: Esta seção descreve como você pode criar seu primeiro módulo no Charles.
---

# Criando seu primeiro módulo

Depois de criar e configurar o seu workspace, agora é necessário adicionar os módulos. 

Um **módulo** é a sua aplicação que está armazenada em um repositório do **Git cadastrado anteriormente**.

## Como adicionar os módulos? 

Para adicionar os módulos, acesse o menu **Modules** __no **workspace** desejado e __preencha os campos:

1. **Nome**: o nome deverá ser a junção da organização e a do módulo, como está no git. Por exemplo: `ZupIt/charlescd`.
2. **URL Git**: informe a URL do seu repositório. Por exemplo: [https://github.com](https://github.com/ZupIT/charlescd).
3. Se seu repositório tem várias aplicações, cadastre-as como **componentes** e informe: 

* **Name of the component**: o nome da aplicação, conforme está no repositório.
* **Métricas:** 

  * **Latency Threshold:** latência \(ms\);
  * **HTTP Error Threshold:**  erro HTTP \(%\). 

  Em ambos os casos, informe um valor de risco que você gostaria de ser alertado caso seu componente alcançasse ou ultrapassasse. 

4. **Add Helm Chart repository:** informe o repositório onde o está o template do helm da sua aplicação e veja o exemplo de como preencher os campos com essa URL:  ****[**https://gitlab.com/charlito/charlescd/moove?ref=master**](https://gitlab.com/charlito/charlescd/moove?ref=master`%20)

* **Insert URL:** [http://gitlab.com](http://gitlab.com);
* **Insert Organization:** charlito; 
* **Insert repository:** charlescd;
* **Insert path:** moove**;** 
* **Insert branch:** master. 

Veja a imagem de como criar módulos: 

![](../../.gitbook/assets/creating-your-first-module.png)

{% hint style="info" %}
Para saber mais sobre a configuração do repositório do helm, [**acesse a seção Configurando o chart template**](configurando-o-chart-template.md)**.** 
{% endhint %}

## O que são componentes? 

{% hint style="info" %}
Componentes são abstrações das aplicações. Se dentro do seu repositório há múltiplas aplicações, cada componente corresponderá a uma delas.
{% endhint %}

