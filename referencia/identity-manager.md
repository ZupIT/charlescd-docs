---
description: >-
  Nesta seção, você encontra detalhes de como funciona o IDM ou Identity
  Manager.
---

# Identity Manager

Na instalação padrão, você tem a opção para que o **próprio Charles gerencie a autenticação dos seus usuários** na plataforma. Entretanto, caso você já tenha um Identity Manager \(IDM\) e queira utilizá-lo, basta fazer a sua configuração durante a instalação do Charles.

## Configurando o IDM

Em`charlescd/install/helm-chart/values.yaml`, altere os valores nos módulos:

### **moove**

```text
internalIdmEnabled: false
```

* `internalIdmEnabled` : Referencia se o moove deve utilizar o IDM padrão ou não. Para informar que você utilizará um IDM externo, informe o valor `false`.

### **ui**

```text
authUri: http://charles.info.example/keycloak
isIdmEnabled: "1"
idmLoginUri: /protocol/openid-connect/auth
idmLogoutUri: /protocol/openid-connect/logout
idmRedirectHost: http://charles.info.example
```

* `authUri`: Armazena o endpoint base do seu IDM que fará a autenticação. Por exemplo, se você fosse utilizar o Google, o valor seria: [`https://accounts.google.com/o/oauth2/v2/auth?`](https://accounts.google.com/o/oauth2/v2/auth?). 
* `isIdmEnabled`: referencia se o UI deverá utilizar um IDM personalizado para realizar a autenticação. Neste caso, 0 indica que utilizará o padrão e 1 um IDM personalizado. 
* `idmLoginUri`: complemento da sua authUri base, é utilizada para indicar qual o endpoint para realizar o login. 
* `idmLogoutUri`: complemento da sua authUri base, é utilizada para indicar qual o endpoint para realizar o logout. 
* `idmRedirectHost`: neste campo é informada a URL do Charles que o usuário deverá ser redirecionado logo após uma atenticação bem sucedida.

### **nginx**

```text
idm:
    endpoint: http://charlescd-keycloak-http/keycloak/auth/realms/charlescd/.well-known/openid-configuration
```

* `idm.endpoint`: o nginx que vem na instalação do Charles fará a validação do token informado durante as requisições, para isso, é necessário informar este endpoint.

