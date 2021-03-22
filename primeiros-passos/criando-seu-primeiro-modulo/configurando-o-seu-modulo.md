---
description: Essa seção descreve como utilizar a lib de propagar o header "X-Circle-Id"
---

# Configurando o seu módulo

## Por que configurar? 

A configuração do módulo é necessária porque, ao trabalhar em cenários com vários microsserviços, você precisa garantir que ocorra a propagação de header `X-Circle-Id` requirida para garantir o [**roteamento das versões corretas**](../../referencia/circulo.md#como-integrar-circulos-com-servicos). Dessa forma, você torna possível que o usuário da sua base chegue na mesma versão de todos os microsserviços que fazem parte do seu teste de hipótese.

Por exemplo, quando você testa uma feature entre microsserviços que tenham integrações em um fluxo de abertura de conta, é necessário garantir que o usuário irá bater em todas as versões corretas que estão no teste de hipótese criado para esse fluxo.

Essa garantia é feita por uma biblioteca de propagação do header X-Circle-Id, que faz com que o [**id do círculo identificado pelo `circle-matcher`**](../../referencia/circle-matcher.md#identificacao-de-circulos-atraves-da-api) seja repassado entre todas as requisições dentro da malha de microsserviços, garantindo que os usuários serão redirecionados para a versão correta do seu teste de hipótese.

{% hint style="info" %}
Caso tenha microsserviços dentro deste fluxo que não fazem parte do seu teste, o valor do círculo será repassado só que a sua requisição cairá em mar aberto, pois não há nenhuma versão destinada para aquele círculo. 
{% endhint %}

### **Exemplo**

Veja abaixo: 

![](../../.gitbook/assets/header-propagation-ptbr-v2.png)

> 1. Ao realizar a chamada de um microsserviço, antes é obtido através do módulo `circle-matcher`o id do círculo que o usuário pertence.
> 2. O id é inserido no header de todas as próximas requisições com a chave `X-Circle-Id`.
> 3. A lib de propagação possibilita repassar o `x-circle-id` no header para a chamada de um outro microsserviço, no caso o `butler`.

No Charles quando acontece um teste de hipótese no `butler`, por exemplo, ele está integrado com o `moove`que é o microsserviço que atende as requisições do frontend. 

Se você quiser que sua requisição chegue na versão correta do `butler`, é preciso que o `moove` repasse o header `X-Circle-Id` \(obtido através de uma requisição para o `circle-matcher`\) nessas requisições feitas para ele. E se envolver mais de um **microsserviço,** é preciso propagar o header para garantir que o usuário tenha a mesma versão daquele círculo.

Quando acontece um teste no `moove`, por exemplo, se ele estiver integrado com o `villager` e o `butler`, a propagação do header `X-Circle-Id` faz com que você procure por versões do `villager` e do `butler` na mesma versão do `moove`, porém como esse não é o cenário, essas requisições entre o `moove` e suas integrações serão tratadas pelo mar aberto.

## Como adicionar? 

O Charles possui uma lib que funciona para qualquer aplicação **Java** que utilize o **Spring** como framework ****e outra para **.NET Core.** As libs foram construídas por não existir nenhuma alternativa amplamente utilizada para esse cenário. 

Para usá-las, você precisa adicioná-las a sua aplicação:

* \*\*\*\*[**Lib para Java e Spring** ](https://github.com/ZupIT/charlescd/tree/master/tracing/spring)\*\*\*\*
* \*\*\*\*[**Lib para .NET**](https://github.com/ZupIT/charlescd/tree/master/tracing/dotnet-core%20)\*\*\*\*

{% hint style="info" %}
Para o **Node.js** já existe uma lib e você pode [**encontrar** **aqui**](https://www.npmjs.com/package/hpropagate). 
{% endhint %}

