---
description: >-
  Esta seção descreve como configurar o Circle Matcher dentro do workspace no
  Charles
---

# Circle Matcher

## Por que configurar?

Quando você  [**cria um workspace**](./), é preciso informar ao Charles para qual Circle Matcher esse workspace atual apontará. É possível que haja um Circle Matcher para cada ambiente, já que o Charles lida, ao mesmo tempo, com diferentes ambientes em vários workspaces. 

Apesar do Circle Matcher ser um módulo independente no Charles, é possível instalá-lo em qualquer área de preferência dentro da arquitetura como, por exemplo, em um cluster público. 

Essa configuração é necessária para que você possa executar operações dentro do Charles, como criar ou editar segmentos de um círculo. 

{% hint style="info" %}
Vale lembrar que, no contexto do Charles, o módulo do Circle Matcher é o que mais recebe solicitações no ambiente por identificar os usuários com base nas regras que você configurou ao [**gerenciar um círculo**](../../referencia/circulos.md#como-criar-circulos). 
{% endhint %}

Se você deseja saber mais sobre o que é um Circle Matcher, [**veja a seção Referência**](../../referencia/circle-matcher.md). 

## Como deve ser configurado

#### Opção 1: Configurar o Circle Matcher em uma arquitetura à parte

Você deve configurar o DNS público que aponta para o Circle Matcher desejado.

> Exemplo: **http://charles.info.example/charlescd-circle-matcher**.



#### Opção 2: Configurar o Circle Matcher no mesmo namespace do Charles 

Caso prefira usar o Circle Matcher no mesmo namespace em que Charles está instalado, você pode usar a mesma referência de DNS. 

A diferença é que, em termos de desempenho, o mais recomendado é usar o nome do serviço do Kubernetes. 

> Exemplo: **http://charlescd-circle-matcher:8080**.

