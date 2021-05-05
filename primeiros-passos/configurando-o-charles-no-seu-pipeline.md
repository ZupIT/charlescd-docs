---
description: >-
  Nesta seção, você encontra os passos para configurar o Charles em seu
  pipeline.
---

# Configurando o Charles no seu pipeline

## Por que integrar o Charles ao seu pipeline de Continuous Delivery?

Integrações são capazes de garantir mais velocidade ao seu time. A entrega contínua \(CD\) permite que você pegue o código armazenado no repositório e o entregue à produção \(ou em qualquer outro ambiente\)  continuamente. 

A configuração de um CD gera um processo mais rápido e eficaz para colocar seu produto no mercado antes da concorrência,  e isso permite que seu time crie novos recursos e correções de bugs para manter seus clientes satisfeitos.

## Pré-requisitos

Para integrar o Charles C.D. no seu pipeline, você precisa saber algumas informações. Veja abaixo quais são e como consegui-las:

* **`x-charles-token`**: É um hash criado quando um **token sistêmico** é gerado. Caso tenha perdido o valor, é possível regerar essa informação. **Veja mais detalhes na seção do token sistêmico**.
* **`x-workspace-id`**: Esse Id representa o workspace onde suas configurações de ambiente e círculos estão. [**Copie o ID no menu existente ao visualizar o workspace**](definindo-workspace/#como-obter-o-identificador-do-meu-workspace)**.**
* **`module.id`**: Esse Id representa o projeto cadastrado no Charles.[ **Copie o ID no menu existente ao visualizar o módulo.**](criando-seu-primeiro-modulo/#como-obter-o-identificador-do-meu-modulo)\*\*\*\*
* **`component.id`**: Esse identificador representa o componente e [**pode ser buscado nos detalhes do módulo**](criando-seu-primeiro-modulo/#como-obter-o-identificador-dos-componentes).
* **`component.version`**: Neste campo informe o nome da tag da imagem do seu componente.
* **`component.artifact`**:  Este é o nome do artefato. Por exemplo: {YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}.
* **`circle.id`**: Esse Id representa o círculo cadastrado no Charles.[ **Copie o ID no menu existente ao visualizar o círculo.**](../referencia/circulo.md#como-obter-o-identificador-do-meu-circulo-1)\*\*\*\*
* **`build.id`**: Este Id representa a composição do deploy criada na primeira requisição[ **citada abaixo**](configurando-o-charles-no-seu-pipeline.md#criar-composicao-do-deploy). Esta informação é retornada como valor da chave `id` na resposta em formato de JSON.

## Como integrar?

Você pode fazer essa integração utilizando um **token sistêmico** e uma sequência de requisições HTTPS. Veja como nos dois passos abaixo:

{% api-method method="post" host="https://charles.info.example" path="/moove/v2/builds/compose" %}
{% api-method-summary %}
Passo 1: Criar composição do deploy
{% endapi-method-summary %}

{% api-method-description %}
Esta requisição cria uma composição que representará sua release em um círculo.  E você pode misturar versões diferentes com vários componentes.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="x-charles-token" type="string" required=false %}
Token sistêmico.
{% endapi-method-parameter %}

{% api-method-parameter name="x-workspace-id" type="string" required=true %}
Identificador do workspace.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="releaseName" type="string" required=true %}
Nome escolhido para a release.
{% endapi-method-parameter %}

{% api-method-parameter name="modules" type="array" required=true %}
Lista de módulos.
{% endapi-method-parameter %}

{% api-method-parameter name="module" type="object" required=true %}
Objeto que representa o módulo.
{% endapi-method-parameter %}

{% api-method-parameter name="module.id" type="string" required=true %}
Identificador do Módulo
{% endapi-method-parameter %}

{% api-method-parameter name="module.components" type="array" required=true %}
Lista dos componentes que irão compor o deploy.
{% endapi-method-parameter %}

{% api-method-parameter name="component" type="object" required=true %}
Componente do módulo.
{% endapi-method-parameter %}

{% api-method-parameter name="component.id" type="string" required=true %}
Identificador do component.
{% endapi-method-parameter %}

{% api-method-parameter name="component.version" type="string" required=true %}
Nome da tag da imagem.
{% endapi-method-parameter %}

{% api-method-parameter name="component.artifact" type="string" required=true %}
Nome do artefato. Por exemplo: {YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
   "id":"0000053e-14cc-4bae-85df-364a70eb0000",
   "author":{
      "id":"00000afe-aa7a-4536-be1b-34eaad4c0000",
      "name":"Charles Admin",
      "email":"admin@email.com"
   },
   "createdAt":"2021-04-19 12:08:54",
   "features":[
      {
         "id":"17c28af1-d7bf-4c8c-9895-ab4944fb5a9e",
         "name":"release-test-0.1",
         "branchName":"release-test-0.1",
         "authorId":"00000afe-aa7a-4536-be1b-34eaad4c0000",
         "authorName":"Charles Admin",
         "modules":[
            {
               "id":"00000df6-6c34-4410-9bea-77ee56900000",
               "name":"ZupIT/charlescd",
               "gitRepositoryAddress":"https://github.com/zupit/charlescd",
               "helmRepository":"{HELM_URL}",
               "createdAt":"2020-11-20 13:11:31",
               "components":[
                  {
                     "id":"00000143-8208-4f95-9986-10b909c00000",
                     "name":"charlescd-ui",
                  },
                  {
                     "id":"000009ea-ada9-40fd-bddf-51c921200000",
                     "name":"charlescd-moove"
                  }
               ]
            }
         ],
         "createdAt":"2021-04-19 12:08:54",
         "branches":[
            "https://github.com/zupit/charlescd/tree/release-test-0.1"
         ]
      }
   ],
   "tag":"release-test-0.1",
   "status":"BUILT",
   "deployments":[]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Veja abaixo um exemplo da requisição no formato **CURL**:

```text
curl 'https://charlescd.api.com/moove/v2/builds/compose' \
  -H 'x-workspace-id: 000009c4-9f58-4236-9936-ffcb04b00000' \
  -H 'x-charles-token: {YOUR_SYSTEM_TOKEN}' \
  -H 'content-type: application/json' \
  --data-binary '{
   "modules":[
      {
         "id":"00000f6-6c34-4410-9bea-77ee56900000",
         "components":[
            {
               "id":"000009ea-ada9-40fd-bddf-51c921200000",
               "version":"{YOUR-TAG-NAME}",
               "artifact":"{YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}"
            },
            {
               "id":"00000143-8208-4f95-9986-10b909c00000",
               "version":"{YOUR-TAG-NAME}",
               "artifact":"{YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}"
            }
         ]
      }
   ],
   "releaseName":"release-test-0.1"
}'
```



{% api-method method="post" host="https://charles.info.example" path="/moove/v2/deployments" %}
{% api-method-summary %}
Passo 2: Implantar a release criada na requisição anterior em um círculo.
{% endapi-method-summary %}

{% api-method-description %}
Esta requisição implanta a release composta criada anteriormente em um círculo.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="x-workspace-id" type="string" required=true %}
Identificador do workspace.
{% endapi-method-parameter %}

{% api-method-parameter name="x-charles-token" type="string" required=true %}
Token sistêmico.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="circleId" type="string" required=true %}
Identificador do círculo que receberá o deployment.
{% endapi-method-parameter %}

{% api-method-parameter name="buildId" type="string" required=true %}
Identificador do build criado na requisição anterior.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
   "id":"0000070c-1ed8-4d33-ad91-04ea50100000",
   "author":{
      "id":"00000afe-aa7a-4536-be1b-34eaad400000",
      "name":"Charles Admin",
      "email":"admin@email.com"
   },
   "createdAt":"2021-04-19 12:08:54",
   "deployedAt":null,
   "circle":{
      "id":"000006b3-6e04-4c48-aca9-c9297e100000",
      "name":"Circle name",
      "author":{
         "id":"00000afe-aa7a-4536-be1b-34eaad400000",
         "name":"Charles Admin",
         "email":"admin@email.com"
      },
      "createdAt":"2021-04-15 17:25:56",
      "matcherType":"REGULAR",
      "rules":{
         "type":"CLAUSE",
         "clauses":[
            {
               "type":"RULE",
               "content":{
                  "key":"email",
                  "value":[
                     "test"
                  ],
                  "condition":"EQUAL"
               }
            }
         ],
         "logicalOperator":"OR"
      },
      "importedAt":null,
      "importedKvRecords":0
   },
   "buildId":"0000053e-14cc-4bae-85df-364a70e00000",
   "tag":"{IMAGE_TAG_NAME}",
   "status":"DEPLOYING",
   "artifacts":[
      {
         "id":"00000652-e4fb-41c7-a6da-33bee0600000",
         "artifact":"{YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}",
         "version":"{IMAGE_TAG_NAME}",
         "createdAt":"2021-04-19 12:08:54",
         "componentName":"charlescd-ui",
         "moduleName":"ZupIT/charlescd"
      },
      {
         "id":"000000d2-4a50-408a-be38-c8e932200000",
         "artifact":"{YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}",
         "version":"{IMAGE_TAG_NAME}",
         "createdAt":"2021-04-19 12:08:54",
         "componentName":"charlescd-moove",
         "moduleName":"ZupIT/charlescd"
      }
   ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Veja um exemplo da requisição no formato **CURL**:

```text
curl 'https://charlescd.api.com/moove/v2/deployments' \
  -H 'x-workspace-id: 000009c4-9f58-4236-9936-ffcb04b00000' \
  -H 'x-charles-token: {YOUR_SYSTEM_TOKEN}' \
  -H 'content-type: application/json' \
  --data-binary '{
   "buildId":"82a7d53e-14cc-4bae-85df-364a70eb9df7",
   "circleId":"1611b6b3-6e04-4c48-aca9-c9297e18d66d"
}'
```

Você pode acompanhar a conclusão dos deployments utilizando os [**Webhooks**](definindo-workspace/web.md).

