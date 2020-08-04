---
description: Esta seção descreve como você deve instalar o Charles no seu projeto.
---

# Instalando o Charles

## Pré-requisitos

Para instalar o Charles, será necessário um ambiente com os seguintes requisitos:

* [**Kubernetes**](https://kubernetes.io/docs/setup/).
* \*\*\*\*[**Istio**](https://istio.io/archive/) ****\(versão &lt;= 1.4 e [_**sidecar injector habilitado**_](https://istio.io/latest/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) no namespace de deploy das suas aplicações\).
* \*\*\*\*[**Prometheus**](https://prometheus.io/docs/prometheus/latest/getting_started/)**,** caso ****queira utilizar [**métricas**](../referencia/metricas/)**.**

## Introdução

O processo de instalação foi criado considerando alguns casos de uso. Logo, para cada um deles, você encontrará um tutorial específico.  Mas, antes veja abaixo quais componentes e as plataformas que o Charles suporta. 

{% hint style="info" %}
Se for necessário instalar o CharlesCD com algumas customizações, sugerimos conferir a seção de [**instalação com helm charts**. ](instalando-charles.md#caso-2-instalacao-com-helm-charts)
{% endhint %}

### Componentes

A instalação do CharlesCD consiste nos seguintes **componentes**:

1. Módulos específicos da[ **arquitetura do Charles.**](../#arquitetura-do-sistema) 
2. **Keycloak**, usado para autenticação e autorização no projeto.

3. Um **banco PostgreSQL** que servirá os módulos de back-end \(`charlescd-moove`, `charlescd-butler` e `charlescd-villager`\) e o Keycloak. 
4. Um **Redis** para uso do `charlescd-villager`.

### Plataforma de Continuous Delivery 

Atualmente, o Charles tem suporte para duas plataformas de Continuous Delivery \(CD\):

* **Spinnaker:** caso você já tenha o seu próprio Spinnaker configurado, ele poderá ser reutilizado.
* **Octopipe:** plataforma nativa, criada pela equipe do CharlesCD para possibilitar uma instalação sem configuração prévia.

{% hint style="info" %}
Você pode saber mais sobre a **configuração do Spinnaker e do Octopipe** na seção [**Configuração de CD**.](../referencia/configuracao-cd.md)
{% endhint %}

## Principais casos de instalação 

No primeiro acesso, **independente do método de instalação**, o usuário padrão de administrador é **charlesadmin@admin** e a senha **charlesadmin.**

### Caso 1: Instalação rápida 

Esta é a instalação mais recomendada para quem nunca usou o CharlesCD  e já quer ter o **primeiro contato em um ambiente de testes,** sem olhar ainda para escalabilidade ou segurança.

Neste caso, você irá utilizar: 

* um arquivo **yaml** com todos os [**componentes**](instalando-charles.md#componentes);
* um **Load Balancer** pré-configurado. 

Para criar esta estrutura, basta executar os arquivos em algum cluster pré-configurado, como minikube, GKE, EKS, etc. Os passos a serem executados são estes:

```text
kubectl create namespace charles

kubectl apply -n charles -f https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml
```

Ao final do processo, você terá dentro do namespace `charles` todos os módulos do projeto e suas dependências instaladas da forma mais simples possível. No link, você encontra os [**arquivos no repositório**](https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml).

###  **Como acessar a aplicação?**

Há duas formas de acessar a aplicação, por meio do **Minikube** ou **Cloud provider**, veja abaixo: 

### **Minikube**

No minikube, o **load balancer nginx não cria automaticamente um IP externo**, para tornar isso possível, siga os passos: 

**Passo 1:** Execute o comando abaixo: 

```text
minikube tunnel
// digite sua senha root, em uma nova aba do terminal e execute:
kubectl get svc -n charles
// agora o IP externo está disponível
```

**Passo 2:** Agora que você tem um ip externo, troque o valor &lt;ip-external-charles&gt; pelo IP externo e adicione essa linha no seu arquivo de host do OS: 

```text
<IP-EXTERNAL-CHARLES>       charles.info.example
```

{% hint style="info" %}
Para mais informações de como mudar o host do seu sistema, [**acesse aqui**](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/). 
{% endhint %}

**Passo 3:** No seu navegador digite http://charles.info.example e a aplicação estará disponível.

### **Cloud Provider \(AWS, GCP, AZURE\)**

Se você instalar em um cluster de kubernetes gerenciado, **o ip externo para o load balancer nginx é criado automaticamente,** siga os passos quando todos os componentes estiverem prontos.

**Passo 1:**  Obtenha o IP externo com o comando:

```text
kubectl get svc -n charles
// get external IP value
```

**Passo 2:** Adicione a linha abaixo no seu arquivo de host do OS \([veja aqui como mudar o host](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/)\) caso você queira acessar do browser da sua máquina:

```text
<IP-EXTERNAL-CHARLES>       charles.info.example
```

{% hint style="info" %}
Se você quiser usar essa instalação em ambientes de produção ou desenvolvimento deverá expor a aplicação usando um DNS.

Depois de apontar seu DNS para o IP externo, faça o clone do single-file.yam e troque todas as ocorrências de http://charles.info.example para o seu novo DNS, depois rode o comando de instalação novamente.

`kubectl install -f <single-file-path> -n charles`
{% endhint %}

{% hint style="danger" %}
Como essa instalação serve **apenas para o uso em ambiente de testes**, não recomendamos esse caso de instalação para ambientes produtivos porque ele não inclui cuidados como: backups do banco de dados, alta disponibilidade, entre outros.
{% endhint %}

### Caso 2: Instalação com helm charts

Esta instalação é indicada para quem possui uma infraestrutura já montada devido a um ambiente mais complexo ou há algumas limitações de segurança e/ou escalabilidade, o que exige uma **customização mais completa da instalação** do CharlesCD.

### Requisitos 

Para realizar o processo, é necessário ter os seguintes programas: 

* \*\*\*\*[**Kubectl**](https://kubernetes.io/docs/tasks/tools/install-kubectl/)\*\*\*\*
* \*\*\*\*[**Helm** ](https://helm.sh/docs/intro/install/)\*\*\*\*

### Como funciona?

Aqui o principal diferencial é a customização. Para isso, foi disponibilizado um **template helm** com todos os campos disponíveis para alteração, incluindo os de banco de dados e recursos consumidos. Você encontra [aqui toda a **documentação dos campos editáveis**.](https://github.com/ZupIT/charlescd/tree/master/install/helm-chart) 

Para realizar a instalação com helm charts, basta executar o comando abaixo depois de customizar os campos:

```text
// customize tudo que precisa no arquivo values.yaml antes de executar o seguinte comando
helm install charlescd <repo-folder> -n <namespace>
```

{% hint style="warning" %}
É importante lembrar que, caso não seja feita nenhuma customização, o resultado final será igual ao do **caso 1** em que, por padrão, instalamos o PostgreSQL, Redis, Keycloak e Octopipe. 

Por isso, não deixe de customizar os campos caso queira algo gerenciável. 
{% endhint %}

