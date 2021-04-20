---
description: 'Nesta seção, você encontra informações sobre webhooks.'
---

# Web

## O que é? 

Webhook é um recurso que observa os eventos de um sistema.  Quando os eventos acontecem, o webhook tem a função de enviar uma notificação para os interessados cadastrados em receber as informações desse evento. 

O Charles possui o módulo**`charlescd-hermes`**, que é responsável por identificar esses eventos e enviar aos inscritos as notificações. 

### Como cadastrar? 

É necessário criar configurações de webhook para utilizar essa funcionalidade no Charle. Para isso, siga os passos abaixo: 

1. Em **Workspace**, clique em **'Add Webhook'**;
2. Preencha os campos de:
   * **Descrição:** adicione a descrição do webhook; 
   * **Webhook URL:** coloque a URL do serviço; 
   * **Secret \(Opcional\):** adicione, se necessário, a chave de aplicação;
3. Escolha o evento: **deploy**, **undeploy** ou **ambos**. 

Depois do cadastro, um card aparecerá com o status de sucesso ou erro da última mensagem enviada. Veja a imagem abaixo: 

![Exemplo de mensagem de erro para solicita&#xE7;&#xE3;o do webhooks](../../.gitbook/assets/image%20%286%29.png)

### **Propriedade comum dos objetos do payload de webhook**

Cada payload de um evento possui propriedades únicas. Você pode encontrá-las nas seções individuais de eventos. Abaixo estão as propriedades comuns:

| Key | Tipo | Descrição |
| :--- | :--- | :--- |
| subscriptionId | String | O id da subscrição do Webhook. |
| executionId | String | O id de execução do evento. Permite rastrear todo ciclo de vida da notificação. |
| event | Object | Detalhes do evento. |

## Eventos

Os eventos observáveis são **início** e **finalização** de **deploy** e **undeploy.** 

### Deploy

Quando você cadastra um webhook para receber informações sobre eventos de **deploy** de um determinado workspace, ou quando um deploy for iniciado e finalizado automaticamente, você irá receber uma notificação com detalhes do evento.

Quando o evento é disparado, um payload HTTP POST é enviado a URL do webhook cadastrado.   
  
Veja abaixo: 

| Key | Tipo | Descrição |
| :--- | :--- | :--- |
| type | String | Tipo do evento. Os valores possíveis são _START\_DEPLOY e FINISH\_DEPLOY_ |
| status | String | Status do evento. Os valores possíveis são _SUCCESS_ e _ERROR_. |
| date | String | Data de execução do evento. |
| timeExecution | Long | Tempo de execução do evento. |
| worspaceId | String | Id do workspace. |
| author | Object | Informações do autor do evento. |
| circle | Object | Informações do círculo do deploy/undeploy. |
| release | String | Informações do deploy/undeploy. |
| features | Object | Informações das features do deploy/undeploy.  |
| error | String | Mensagem de erro em caso de status com erro. |

### Exemplo de payload do evento **de Deploy**

```text
{
  "subscriptionId": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
  "executionId": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
  "event": {
    "type": "FINISH_DEPLOY",
    "status": "FAIL",
    "error": "Failed to pull image nexus.mydomain.co.uk/nginx Error: image nginx:latest not found",
    "date": "2020-01-10 22:00:00",
    "workspaceId": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
    "author": {
      "email": "charlescd@zup.com.br",
      "name": "CharlesCD"
    },
    "circle": {
      "id": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
      "name": "circle-qas"
    },
    "release": {
      "tag": "tag",
      "modules": [
        {
          "id": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
          "name": "ZupIt/charlescd",
          "componentes": [
            {
              "id": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
              "name": "charlescd-moove"
            },
            {
              "id": "5d4c95b4-6f83-11ea-bc55-0242ac130004",
              "name": "charlescd-villager"
            }
          ]
        },
        {
          "id": "5d4c95b4-6f83-11ea-bc55-0242ac130004",
          "name": "ZupIt/horusec",
          "componentes": [
            {
              "id": "5d4c95b4-6f83-11ea-bc55-0242ac130005",
              "name": "horusec-account"
            },
            {
              "id": "5d4c95b4-6f83-11ea-bc55-0242ac130006",
              "name": "horusec-analytics"
            }
          ]
        }
      ],
      "features": [
        {
          "name": "new-moove-feature",
          "branchName": "feature/moove-feature"
        },
        {
          "name": "new-horusec-feature",
          "branchName": "feature/horusec-feature"
        }
      ]
    }
  }
}
```

###  Undeploy

Quando você cadastra um webhook para receber informações sobre eventos de **undeploy,** de um determinado workspace, quando um undeploy for iniciado e finalizado automaticamente você irá receber uma notificação com detalhes do evento.

Quando o evento é disparado, um payload HTTP POST é enviado a URL do webhook cadastrado, veja abaixo: 

| Key | Tipo | Descrição |
| :--- | :--- | :--- |
| type | String | Tipo do evento. Os valores possíveis são _START\_UNDEPLOY e FINISH\_UNDEPLOY_ |
| status | String | Status do evento. Os valores possíveis são _SUCCESS_ e _ERROR_. |
| date | String | Data de execução do evento. |
| worspaceId | String | Id do workspace. |
| author | Object | Informações do autor do evento. |
| circle | Object | Informações do círculo do deploy/undeploy. |
| release | String | Informações do deploy/undeploy. |
| features | Object | Informações das features do deploy/undeploy.  |
| error | String | Mensagem de erro em caso de status com erro. |

### Exemplo de payload do evento **de Undeploy**

```text
{
  "subscriptionId": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
  "executionId": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
  "event": {
    "type": "START_UNDEPLOY",
    "status": "SUCCESS",
    "date": "2020-01-10 22:00:00",
    "workspaceId": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
    "author": {
      "email": "charlescd@zup.com.br",
      "name": "CharlesCd"
    },
    "circle": {
      "id": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
      "name": "circle-qas"
    },
    "release": {
      "tag": "tag",
      "modules": [
        {
          "id": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
          "name": "ZupIt/charlescd",
          "componentes": [
            {
              "id": "5d4c95b4-6f83-11ea-bc55-0242ac130003",
              "name": "charlescd-moove"
            },
            {
              "id": "5d4c95b4-6f83-11ea-bc55-0242ac130004",
              "name": "charlescd-villager"
            }
          ]
        },
        {
          "id": "5d4c95b4-6f83-11ea-bc55-0242ac130004",
          "name": "ZupIt/horusec",
          "componentes": [
            {
              "id": "5d4c95b4-6f83-11ea-bc55-0242ac130005",
              "name": "horusec-account"
            },
            {
              "id": "5d4c95b4-6f83-11ea-bc55-0242ac130006",
              "name": "horusec-analytics"
            }
          ]
        }
      ],
      "features": [
        {
          "name": "new-moove-feature",
          "branchName": "feature/moove-feature"
        },
        {
          "name": "new-horusec-feature",
          "branchName": "feature/horusec-feature"
        }
      ]
    }
  }
}
```

