---
description: 'Nesta seção, você encontra como acessar o Charles.'
---

# Seu primeiro acesso ao Charles

{% hint style="info" %}
No primeiro acesso, **independente do método de instalação**, o usuário padrão de administrador é **charlesadmin@admin** e a senha **charlesadmin.** 

Logo depois de fazer o primeiro login, troque essa senha. 
{% endhint %}

Há **três formas de acessar a aplicação:** 

* **Minikube.**
* **Microk8s.** 
* **Cloud provider.** 

Veja abaixo como funciona a configuração para cada uma delas.

## **1. Minikube**

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

## 2. Microk8s

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

## **3. Cloud Provider \(AWS, GCP, AZURE\)**

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
Caso queira usar essa instalação em ambientes de produção ou desenvolvimento, deverá expor a aplicação usando um DNS.

Depois de apontar seu DNS para o IP externo, faça o clone das configurações \(seja o singlefile ou os arquivos do helm\) e troque todas as ocorrências de http://charles.info.example para o seu novo DNS, depois execute o comando de instalação novamente.
{% endhint %}

