---
description: Esta seção descreve como você deve instalar o Charles no seu projeto.
---

# Instalando o Charles

## Pré-requisitos

Para instalar o Charles, será necessário um ambiente com os seguintes requisitos:

* [**Kubernetes**](https://kubernetes.io/docs/setup/).
* \*\*\*\*[**Istio**](https://istio.io/archive/) ****\(versão &lt;= 1.4 e [_**sidecar injector habilitado**_](https://istio.io/latest/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) no namespace de deploy das suas aplicações\).
* \*\*\*\*[**Prometheus**](https://prometheus.io/docs/prometheus/latest/getting_started/)**,** caso ****queira utilizar [**métricas**](../referencia/metricas/)**.**

### Recursos mínimos

Os recursos mínimos considerando apenas a instalação do Charles são:

* **Microk8s**: 2GB de RAM; 
* **Minikube**: 4GB de RAM;
* **Cluster**: 2GB de RAM.

## Introdução

O processo de instalação foi criado considerando alguns casos de uso. Logo, para cada um deles, você encontrará um tutorial específico.  Mas, antes veja abaixo quais componentes e as plataformas que o Charles suporta. 

{% hint style="info" %}
Se for necessário instalar o CharlesCD com algumas customizações, sugerimos conferir a seção de [**instalação com helm charts**. ](instalando-charles.md#caso-2-instalacao-com-helm-charts)
{% endhint %}

### Componentes

A instalação do CharlesCD consiste nos seguintes **componentes**:

1. Módulos específicos da[ **arquitetura do Charles**](../#arquitetura-do-sistema)**.**
2. **Keycloak**, usado para autenticação e autorização no projeto.
3. Um **banco PostgreSQL** que servirá os módulos de backend \(`moove`, `butler` e `villager`\) e o Keycloak.
4. Um **Redis** para uso do `charlescd-circle-matcher`
5. **Octopipe** está na instalação padrão do Charles como uma opção de CD, além de acompanhar um **mongoDB** para ****uso do mesmo. Entretanto, essa ferramenta é opcional, é possível desabilitá-la. 

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

Há **três formas de acessar a aplicação:** por meio do **Minikube,** **Microk8s** ou **Cloud provider.** Veja abaixo como funciona a configuração para cada uma delas: 

### **1. Minikube**

No minikube, o **load balancer nginx não cria automaticamente um IP externo.** Para tornar isso possível, siga os passos: 

**Passo 1:** Execute o comando abaixo

```text
minikube tunnel
// digite sua senha root, em uma nova aba do terminal e execute:
kubectl get svc -n charles
// agora o IP externo está disponível
```

**Passo 2:** Agora que você tem um IP externo, troque o valor &lt;ip-external-charles&gt; pelo IP externo e adicione essa linha no seu arquivo de host do OS: 

```text
<IP-EXTERNAL-CHARLES>       charles.info.example
```

{% hint style="info" %}
Para mais informações de como mudar o host do seu sistema, [**acesse aqui**](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/). 
{% endhint %}

**Passo 3:** Por fim, digite no seu navegador **http://charles.info.example** e a aplicação estará disponível.

### 2. Microk8s

O Microk8s está disponível para as Plataformas do Microsoft Windows, Apple MacOS e para Linux. 

{% hint style="info" %}
Para mais informações sobre como instalar o **Microk8s**, [**visite o site do projeto**](https://microk8s.io). 
{% endhint %}

Uma vez instalado o Microk8s, você deve habilitar os seguintes add-ons:

* **DNS**: Descobrimento dos serviços dentro do cluster;
* **Storage:** Criação de volumes e persistências das PODs;
* **MetalLB:** Acesso aos serviços expostos pelo Kubernetes. Para este add-on, você terá que escolher um range de IPs onde o Load Balancer irá atribuir para a exposição dos seus serviços.

Siga os passos abaixo para habilitar o Microk8s no Charles: 

**Passo 1:** Prepare o Microk8s para receber o CharlesCD.

```text
microk8s enable dns storage metallb
Enabling DNS
Applying manifest
serviceaccount/coredns created
configmap/coredns created
deployment.apps/coredns created
service/kube-dns created
clusterrole.rbac.authorization.k8s.io/coredns created
clusterrolebinding.rbac.authorization.k8s.io/coredns created
Restarting kubelet
DNS is enabled
Enabling default storage class
deployment.apps/hostpath-provisioner created
storageclass.storage.k8s.io/microk8s-hostpath created
serviceaccount/microk8s-hostpath created
clusterrole.rbac.authorization.k8s.io/microk8s-hostpath created
clusterrolebinding.rbac.authorization.k8s.io/microk8s-hostpath created
Storage will be available soon
Enabling MetalLB
Enter each IP address range delimited by comma 
(e.g. '10.64.140.43-10.64.140.49,192.168.0.105-192.168.0.111'):
// Nesta etapa, você pode escolher um range ou usar o sugerido, vamos utilizar
// 10.64.140.43-10.64.140.49
10.64.140.43-10.64.140.49
Applying registry manifest
namespace/metallb-system created
podsecuritypolicy.policy/speaker unchanged
serviceaccount/controller created
serviceaccount/speaker created
clusterrole.rbac.authorization.k8s.io/metallb-system:controller unchanged
clusterrole.rbac.authorization.k8s.io/metallb-system:speaker unchanged
role.rbac.authorization.k8s.io/config-watcher created
clusterrolebinding.rbac.authorization.k8s.io/metallb-system:controller unchanged
clusterrolebinding.rbac.authorization.k8s.io/metallb-system:speaker unchanged
rolebinding.rbac.authorization.k8s.io/config-watcher created
daemonset.apps/speaker created
deployment.apps/controller created
configmap/config created
MetalLB is enabled

// Com o microk8s configurado, podemos então instalar o Charles usando
// o single-file
microk8s.kubectl create namespace charles
​​microk8s.kubectl apply -n charles -f https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml

// agora o IP externo está disponível
microk8s.kubectl get svc -n charles

```

**Passo 2:** Agora que você tem um IP externo, troque o valor **&lt;ip-external-charles&gt;** pelo IP externo e adicione a linha no seu arquivo de host do OS: 

```text
<IP-EXTERNAL-CHARLES>       charles.info.example
```

{% hint style="info" %}
Para mais informações sobre como mudar o **host file**, [**acesse aqui**](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/). 
{% endhint %}

**Passo 3:** Digite no seu navegador **http://charles.info.example** e a aplicação estará disponível.

### **3. Cloud Provider \(AWS, GCP, AZURE\)**

No caso do Cloud Provider, você pode criar automaticamente o **IP externo para o load balancer nginx**  ao fazer sua instalação em um cluster de kubernetes gerenciado. 

Siga os passos quando todos os componentes estiverem prontos.

**Passo 1:**  Obtenha o IP externo com o comando.

```text
kubectl get svc -n charles
// get external IP value
```

**Passo 2:** Adicione a linha abaixo no seu arquivo de host do OS \([**veja aqui como mudar o host**](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/)\) caso você queira acessar do browser da sua máquina:

```text
<IP-EXTERNAL-CHARLES>       charles.info.example
```

{% hint style="info" %}
Se você quiser usar essa instalação em ambientes de produção ou desenvolvimento, deverá expor a aplicação usando um DNS.

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

## Autenticação do cluster com o seu registry

### Por que autenticar?

A autenticação é necessária para os casos em que você utiliza um registry privado. Com isso, é possível o cluster se comunicar com o seu [**registry**](definindo-workspace/docker-registry.md) quando for preciso fazer o pull das imagens. 

### Como autenticar?

O cluster Kubernetes utiliza o tipo **secret** do **docker-registy** para autenticar o registry container. 

{% hint style="info" %}
 Para mais informações de como gerar esse **secret** para ser ****aplicado no seu cluster, [**acesse a documentação do Kubernetes**](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/). 
{% endhint %}

Uma vez gerado o secret, ele ter uma formatação parecida com o exemplo abaixo:

```text
apiVersion: v1
data:
  .dockerconfigjson: <<your value>>
kind: Secret
metadata:
  name: <<registry-name>>
type: kubernetes.io/dockerconfigjson
```

Com o secret em mãos, você precisará aplicar essa informação no namespace onde suas aplicações serão implantados pelo Charles:

```text
kubectl -n your-namespace apply secret-registry.yaml
```

Finalizando desses passos, seu cluster consiguirá manter a comunicação com o registry.
