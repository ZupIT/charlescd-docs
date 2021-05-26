---
description: >-
  In this section, you will find details on IDM (Identity Manager)
  configuration.
---

# Identity Manager

On the default installation, you have the option which **Charles manages your users' authentications** on the platform. However, if you already have an Identity Manager \(IDM\) and want to use it, just configure it during Charles' installation. 

## Configuring IDM

On`charlescd/install/helm-chart/values.yaml`, change the values on the modules:

### **moove**

```text
internalIdmEnabled: false
```

* `internalIdmEnabled` : Refers whether moove must use the default IDM or not. To inform that you will use an external IDM, add the value`false`.

### **ui**

```text
authUri: http://charles.info.example/keycloak
isIdmEnabled: "1"
idmLoginUri: /protocol/openid-connect/auth
idmLogoutUri: /protocol/openid-connect/logout
idmRedirectHost: http://charles.info.example
```

* `authUri`: Stores the base endpoint of your IDM that will make authentication. For example, if you were going to use Google, the value would be:  [`https://accounts.google.com/o/oauth2/v2/auth?`](https://accounts.google.com/o/oauth2/v2/auth?). 
* `isIdmEnabled`: refers if the customized UI will make the authentication. In this case, 0 indicates that it will use the default and 1 a customized IDM. 
* `idmLoginUri`:  it is your authUri base complement,  it's used to indicate which endpoint will log in.  
* `idmLogoutUri`:  it is your authUri base complement,  it's used to indicate which endpoint will logout. 
* `idmRedirectHost`: this field you inform Charles' URL where the user will be redirected right after successful authentication. 

### **nginx**

```text
idm:
  endpoint: http://charlescd-keycloak-http/
	port: 443
	path: keycloak/auth/realms/charlescd/.well-known/openid-configuration
```

* `idm.endpoint:` this field represents the hostname of the IDM that you're using with Charles.
* `idm.port:` this field represents the port of the IDM that you're using with Charles. 
* `idm.path:` this field represents the path that will be used by the envoy to validate your token. For this token validation, Charles uses the openID /userinfo endpoint.

 

