# Ações

## O que é? 

Depois de [**cadastrar seu grupo de métricas**](../../referencia/metricas/grupo-de-metricas.md), o Charles mostra o acompanhamento dessas métricas e oferece ações para cada uma delas. Ação é um tipo de trigger que será disparado quando todos os limites \(thresholds\) são alcançados.

## Como configurar? 

Em configurações do workspace, clique na seção **Add Metric Action** e siga os passos:

**1. Add action configuration**: Adicione uma ação de configuração;  
**2. Type a nickname:** Escreva um nome para sua action;  
**3. Type a description:** Descreva o sua action;  
**4. Select a plugin:** Selecione um plugin para executar a ação.

![](../../.gitbook/assets/usandoactions-metricas%20%281%29.gif)

{% hint style="info" %}
O único plugin disponível no momento é o **circle deployment**. Ele permite que o Charles faça o seu próprio plugin para atender às necessidades da sua aplicação como, por exemplo, uma action que envie e-mail para avisar o status do círculo.
{% endhint %}

Para mais informações sobre **Action**, veja a [**seção de Referência**](../../referencia/metricas/acoes.md). 

