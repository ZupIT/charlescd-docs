---
description: 'Nesta seção, você encontra como instalar o Charles via Single File.'
---

# Instalando via Single File

{% hint style="info" %}
Antes de prosseguir, tenha certeza de que todos os [**pré-requisitos**](./#pre-requisitos) estão devidamente instalados.
{% endhint %}

Esta é a instalação mais recomendada para quem nunca usou o CharlesCD e já quer ter o **primeiro contato em um ambiente de testes,** sem olhar ainda para escalabilidade ou segurança.

Neste caso, você irá utilizar: 

* um arquivo **yaml** com todos os [**componentes**](./#componentes);
* um **Load Balancer** pré-configurado. 

### Como instalar?

{% hint style="danger" %}
Esta instalação utiliza senhas padrões que podem ser encontradas no nosso repositório. Para trocá-las, opte pela [**instalação via helm**](instalando-via-helm.md) onde você pode realizar a troca das senhas.
{% endhint %}

Para criar esta estrutura, basta executar os arquivos em algum cluster pré-configurado, como Minikube, GKE, EKS, etc. Os passos a serem executados são estes:

```text
kubectl create namespace charles

kubectl apply -n charles -f https://raw.githubusercontent.com/ZupIT/charlescd/main/install/helm-chart/single-file.yaml
```

Ao final do processo, você terá dentro do namespace `charles` todos os módulos do projeto e suas dependências instaladas. No link, você encontra os [**arquivos no repositório**](https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml).

{% hint style="info" %}
Se você quiser usar essa instalação em ambientes de produção ou desenvolvimento, deverá expor a aplicação usando um DNS.

Depois de apontar seu DNS para o IP externo, faça o clone do single-file.yam e troque todas as ocorrências de http://charles.info.example para o seu novo DNS, depois rode o comando de instalação novamente.

`kubectl install -f <single-file-path> -n charles`
{% endhint %}

{% hint style="danger" %}
Como essa instalação serve **apenas para o uso em ambiente de testes**, não recomendamos esse caso de instalação para ambientes produtivos porque ele não inclui cuidados como: backups do banco de dados, alta disponibilidade, entre outros.
{% endhint %}

