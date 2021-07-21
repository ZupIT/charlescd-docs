---
description: 'Nesta seção, você encontra informações sobre as ações de métricas.'
---

# Ações de métricas

## O que é? 

Depois de [**cadastrar seu grupo de métricas**](../../referencia/metricas/grupo-de-metricas.md), o Charles mostra o acompanhamento dessas métricas e oferece ações para cada uma delas. Ação é um tipo de trigger que será disparado quando todos os limites \(thresholds\) são alcançados.

## Como configurar? 

Em configurações do workspace, clique na seção **Add Metric Action** e siga os passos:

**1. Add action configuration**: Adicione uma ação de configuração;  
**2. Type a nickname:** Escreva um nome para sua action;  
**3. Type a description:** Descreva o sua action;  
**4. Select a plugin:** Selecione um plugin para executar a ação.

![](../../.gitbook/assets/workspace_metricaction%20%282%29.gif)

{% hint style="info" %}
Os plugins disponíveis são **circle deployment** e **circle undeployment**. O Charles pode fazer o  próprio plugin para atender às necessidades da sua aplicação como, por exemplo, uma action que envie e-mail para avisar o status do círculo.
{% endhint %}

Dentro do seu círculo, você deve registrar seu ****[**grupo de métricas**](../../referencia/metricas/grupo-de-metricas.md) que é responsável por criar métricas que você  deseja acompanhar e você também pode adicionar ações nesse grupo. Quando chegar ao ponto limite \(threshold\), a ação será chamada e o que está descrito no seu plugin irá acontecer, como por exemplo, um deployment de um círculo dentro de outro círculo.

Para mais informações sobre **Action**, veja a [**seção de Referência**](../../referencia/metricas/acoes.md). 

