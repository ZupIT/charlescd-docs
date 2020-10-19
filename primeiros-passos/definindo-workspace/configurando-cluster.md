---
description: Esta seção descreve como funciona o ambiente de deploy no Charles.
---

# Ambiente de deploy

Ao configurar seu workspace é preciso cadastrar as credenciais de acesso ao cluster [**Kubernetes**](https://kubernetes.io/). Essas configurações são específicas para cada uma das ferramentas de _Continuous Deployment_ \(CD\) que são integradas ao CharlesCD. No momento, damos suporte ao  [**Spinnaker**](https://www.spinnaker.io/) e **CharlesCD** \(Octopipe\). 

{% hint style="info" %}
O **Charles** possui um módulo chamado Octopipe que é uma forma leve e de baixo custo de fazer deploy em clusters Kubernetes.
{% endhint %}

### Como realizar seu deploy?

Veja abaixo o exemplo de como realizar seu deploy utilizando o CharlesCD no mesmo cluster de instalação:

1. Clique em **Add CD Configuration**;
2. Selecione a opção **CharlesCD.**

Após esses passos, preencha os campos a seguir:

1. **Name:** nome da configuração que será criada.
2. **Namespace:** defina o namespace que será utilizado nos deploys no cluster _Kubernetes._
3. **Git provider**: defina o provedor de git a ser utilizado \(**GitHub ou GitLab**\).
4. **Git token:** insira um token de autenticação que tenha acesso repositório git onde está armazenado os seus [**templates Helm**](../criando-modulos/configurando-o-chart-template.md) ****que serão utilizados durante o deployment da sua [**aplicação**](../criando-modulos/). Caso o seu Git Provider seja **GitHub**, é necessário a permissão "_repo_".  Se for o contrário, configure no **GitLab** os acessos: "_api_ '' e "_read\_repository_".
5. Selecione a opção **Default**.

Depois de finalizar sua configuração, você pode futuramente associá-la a um módulo. Para mais informações, acesse [**Configurações de CD**.](../../referencia/configuracao-cd.md)

