---
description: >-
  Nesta seção, você encontra detalhes sobre o grupo de métricas para monitorar a
  saúde do seu software.
---

# Grupo de métricas para monitorar a saúde

## O que é?

Quando sua aplicação já fez o deployment, é uma boa prática checar a saúde, confirmar se o software está funcionando bem ou observar se precisa de algum cuidado específico. 

Você pode fazer isso com o Charles, utilizando o [**datasource configurado anteriormente**](../../primeiros-passos/definindo-workspace/adicionando-o-datasource.md) ****para criar um grupo de métricas especifico que informa o status da sua imagem em que foi feito o deploy. 

 Você encontra abaixo os detalhes.

## Requisitos

Para monitorar suas métricas, é preciso ter: 

*  Datasource configurado e usando o [**Prometheus**](https://prometheus.io/)
*  [**Istio**](https://istio.io/latest/) ****1.7 ou versões mais recentes.

## Como monitorar?

1. Você precisar criar seu **grupo de métricas** e **sua própria métrica**, para fazer isso [**siga os passos aqui**](grupo-de-metricas.md);
2. No modo avançado, execute o **PromQL queries** para monitorar suas métricas. ****

{% hint style="info" %}
Para saber mais sobre o **PromQL**, veja a [**documentação**](https://prometheus.io/docs/prometheus/latest/querying/basics/).
{% endhint %}

## Exemplos de métricas

Depois de ter criado o seu grupo de métricas, você também pode criar suas própria métrica. Veja alguns exemplos abaixo.

{% hint style="info" %}
Quando você utiliza algum desses exemplos, você pode criar várias métricas, como a proporção de erro pelo total de requisições e você também pode disparar uma ação se essa métrica atingir 10%. 
{% endhint %}

### Métrica de latência 

Aqui, a métrica foi criada para oferecer uma média de latência. Ela divide a duração \(em milissegundos\) das requisições no último minuto pelo total de requisições no mesmo tempo. 

No final da query, o valor é multiplicado por 1000 e você consegue ver o resultado em segundos.

{% hint style="warning" %}
Mude o valor de **xyz**  em uma query usando o valor do Circle ID. É válido para todos os exemplos abaixo. ****
{% endhint %}

```
round (
 ( sum (
     irate (
       istio_request_duration_milliseconds_sum{circle_source="xyz"}[1m]
     )
   ) by(destination_component) /
   sum (
     irate (
       istio_request_duration_milliseconds_count{circle_source="xyz"}[1m]
     )
   ) by(destination_component)
 ) 
   * 1000
)
```

{% hint style="info" %}
Depois que você instalou o Istio e Prometheus e habilitou o Istio injection em seus pods,  o seu Datasource terá as métricas do Istio. Essas métricas podem ser usadas para construir outras mais avançadas. Veja mais sobre essa métrica na [**documentação** **Istio Standard Metrics**](https://istio.io/latest/docs/reference/config/metrics/)**.**
{% endhint %}

### Métrica do total de requisições 

Essa métrica oferece o total de requisições no último minuto: 

```
ceil (
  sum (
    irate (
      istio_requests_total{circle_source="xyz"}[1m]
    )
  ) 
  by(destination_component)
) 
```

### Métricas de erro de requisção

Essa métrica mostra quais as requisições no último minuto, mas apenas para a status de resposta com o código 404: 

```
ceil (
  sum (
    irate (
      istio_requests_total{circle_source="xyz",
      response_code="404"}[1m]
    )
  ) 
  by(destination_component)
) 
```

{% hint style="warning" %}
Você precisa definir um limite \(threshold\) que preencha o que você precisa monitorar. 
{% endhint %}

