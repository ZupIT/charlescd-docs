---
description: >-
  Nesta seção, você encontra detalhes sobre como funcionam os círculos no
  Charles.
---

# Círculo

Os círculos são o principal diferencial do [**novo conceito de deploy** ](../faq/sobre-charles.md#o-que-e-deploy-em-circulos)trazido pelo Charles. Ele possibilita a criação de grupos de usuários a partir de diversas características e, dessa forma, promove testes simultâneos de aplicações para o maior número possível de usuários.

![Representa&#xE7;&#xE3;o dos c&#xED;rculos gerados no Charles](../.gitbook/assets/deploy_em_circulos%20%288%29%20%281%29.png)

Além de indicar as segmentações de clientes, os círculos também auxiliam na gestão de versões implantadas para este público.

Uma vez escolhidas as pessoas certas para terem acesso à sua release associada ao círculo, o Charles irá gerar uma [**série de métricas** ](metricas/)de negócio ou desempenho. Essas informações te darão maior visibilidade dos resultados de uma hipótese ou feature em análise, possibilitando testes mais assertivos.

## Como criar círculos?

Para você criar um círculo, siga os seguintes passos:

**1.** Clique em **Create Circle**.  
**2.** Dê um nome ao seu círculo.  
**3.** Defina uma **segmentação**.  
**4.** \[Opcional\] Implante uma release.

## O que é uma segmentação? 

As segmentações são um **conjunto de características ou um valor percentual**  que você define para agrupar seus usuários nos círculos. Existem três maneiras de segmentar seus usuários: 

1. Por meio do **preenchimento de regras de forma manual.**
2. Por meio da **importação de um arquivo CSV**.
3. Por meio de um **valor de porcentagem em relação ao total de acessos à aplicação** 

### Campos de uma segmentação 

As segmentações manuais possuem os seguintes campos: 

* **Chave**: é o mesmo valor presente como chave _payload_ da requisição de identificação do usuário.
* **Condição**: é a implicação lógica que condicionará sua chave e seu valor.
* **Valor**: são os valores existentes na sua base que poderão ser utilizados para compor a lógica de segmentação.

#### Chave e valor

Os campos **chave** e **valor** são estabelecidos com base nas informações que serão enviadas na requisição que [**identifica os círculos**](circle-matcher.md#identificando-circulos-atraves-do-charlescd) aos quais o seu usuário pertence. Por exemplo, considere que o seguinte payload representa as informações que você possui do seu cliente:

```text
{
  "id": "7f2926d5-ff08-4d49-96df-d4ba0fc07b52",
  "name": "Alice",
  "state": "MG",
  "city": "Uberlândia",
  "age": "47",
  "groupId": "a435bd12-ae82-48c8-b164-066d91ffe3a5"
}
```

As chaves utilizadas podem ser qualquer uma enviada no payload da sua aplicação ao circle-matcher do Charles, como: **id**, **name**, **state**, **city**, **age** e **groupId**. 

{% hint style="warning" %}
**O seu payload e as chaves devem ser iguais.**
{% endhint %}

### Porcentagem

A segmentação por porcentagem possui o seguinte campo: 

* **Porcentagem**: valor que indica o percentual \(%\) das requisições que serão direcionadas para um círculo. Por exemplo, em um cenário que exista um circulo com porcentagem de 10%,  a cada 100 requisições, aproximadamente 10 serão direcionadas para o círculo. 

{% hint style="warning" %}
A soma dos fatores dos círculos ativos com segmentação por porcentagem nunca deve ultrapassar 100.

Caso seja igual a 100, significa que o círculo **Default** jamais será indicado pelo Circle Matcher.
{% endhint %}

Esse direcionamento é feito somente para usuários que pertencem ao círculo **Default** ou aos círculos com **segmentação de porcentagem**.  Os usuários que pertencem ao círculos com segmentação por regras, seja manual ou por CSV, nunca serão direcionados para círculos com segmentação por porcentagem.

Se na sua configuração existir círculos com segmentação por regras e círculos com segmentação por porcentagem, veja abaixo a lógica de identificação no Circle Matcher:

1. Verifica se o payload faz _match_ com algum círculo de segmentação por regras. Caso positivo, este\(s\) círculo\(s\) será\(ão\) retornado\(s\) e a busca pelos círculos é finalizada.
2. Se não encontrar nenhum círculo compatível e existir círculos ativos com segmentação por porcentagem,  um número aleatório entre 1 e 100 é sorteado e se ele for menor ou igual ao fator do círculo, este é retornado.
3. Caso nenhum dos passos anteriores encontre um círculo compatível, o id do círculo **Default** é retornado.

### Exemplo de criação de círculo

Veja abaixo um exemplo de como criar um círculo: 

![Como criar c&#xED;rculos](../.gitbook/assets/circle_create_segmentation.gif)

{% hint style="info" %}
Uma **grande vantagem de utilizar as segmentações** é a possibilidade fazer combinações lógicas entre vários atributos para criar diferentes categorias de públicos e, dessa forma, utilizá-los nos testes das hipóteses.   
  
Por exemplo, a partir das características “_age_” e “_state_”, é possível criar círculos por faixas etárias por região.
{% endhint %}

### **Segmentação manual**

Nesta segmentação, você define as lógicas que o círculo deve seguir para compor uma combinação com usuários que atendam às características pré-determinadas.

Essas características podem ser definidas com base nas lógicas de:

* Equal to
* Not Equal
* Lower Than
* Lower or equal to
* Higher than
* Higher or equal to
* Starts With

Veja alguns exemplos:

![](../.gitbook/assets/segmentacao-manual%20%281%29.png)

### **Segmentação por importação de CSV**

Nessa modalidade, é utilizada apenas a primeira coluna do CSV para criar as regras. Sendo assim, a primeira linha da primeira coluna deve conter o nome da chave e a mesma deve ser informada no campo **key**_:_

![Exemplo de importa&#xE7;&#xE3;o por CSV ](../.gitbook/assets/chrome-capture-5-.jpg)

Depois de ter feito o upload do arquivo e salvado as configurações, aparecerá um overview demonstrando como está sua segmentação:

![Overview](../.gitbook/assets/image%20%284%29.png)

Essa segmentação permite, por exemplo, extrair de uma base externa de IDs dos clientes um perfil específico e importá-los direto na plataforma do Charles. Quando um arquivo .csv é importado e se ele conter alguma linha em branco, ocorrerá um erro da importação, pois não é permitido a criação de segmentos dessa forma.

{% hint style="warning" %}
O único operador lógico suportado nesta segmentação é o OR \(Ou\).
{% endhint %}

### **Segmentação por porcentagem**

É o tipo de segmento que distribui aos círculos a quantidade total de requisições que não foram filtradas em alguma segmentação manual. Essas requisições são entregues, de maneira proporcional, entre o círculos configurados para essa segmentação e o círculo default.

O valor da porcentagem para cada círculo é definido entre 0 e 100, e a soma de todos os círculos ativos não pode ultrapassar 100%.

#### Exemplos de segmentação por porcentagem

Supondo que você criou dois círculos com porcentagem: 

* O círculo **A,** com 15%
* O círculo **B,** com 26%.

A partir daí, o algoritmo para identificação sorteia um número entre 1 e 100 \(inclusive\), e em seguida, é feita a seguinte análise:

1. Se o número for menor ou igual a 15, é retornado o círculo **A**.
2. Se o número for maior que 15 e menor ou igual a 41 \(15 + 26\), é retornado o círculo **B**.
3. Se o número for maior que 41, é retornado o círculo **Default**.

Se não houver nenhum círculo configurado ou ativo, a quantidade disponível será de 100%, como na imagem abaixo:

![](../.gitbook/assets/perc1.png)

Se você, por exemplo, possui três círculos ativos por porcentagem e cada um tem o valor de 30% , a quantidade disponível para seu novo círculo será de 10%. Veja abaixo:  

![](../.gitbook/assets/perc2.png)

Depois que a segmentação é criada, o percentual disponível só será alterado caso uma release seja implantada para aquele círculo e ele se torne ativo.

![](../.gitbook/assets/perc3.png)

Se, por exemplo, **a porcentagem** **atingir os 100% disponíveis,**  é necessário alterar ou remover os círculos ativos e configurados para que haja espaço para você criar um novo círculo.

![](../.gitbook/assets/perc4.png)



### Como obter o identificador do meu círculo?

Assim que seu círculo é criado, mesmo sem a definição das configurações, ele já possui um identificador único. 

Para obter essa informação, siga estes passos: 

1. Selecione o círculo
2. Clique em "default" 
3. E, no menu à esquerda, clique em **Copy ID**

![](../.gitbook/assets/como-obter-o-identificador-do-meu-circulo.gif)

## Círculos ativos e inativos

O que define se um círculo é ativo ou não, é a existência de [**releases**](release.md), isto é, de versões implantadas para aquela segmentação de usuários. Por isso, os círculos ativos são os que possuem releases implantadas, enquanto os círculos inativos ainda não possuem nenhuma.

![](../.gitbook/assets/circulo-ativo-e-inativo%20%281%29.gif)

## Como integrar círculos com serviços?

Uma vez detectado o [**círculo ao qual o usuário pertence**,](circle-matcher.md#identificacao-de-circulos-atraves-da-api) essa informação deve ser repassada para todas as próximas requisições através do parâmetro **`x-circle-id`** no header. Isso acontece porque o Charles detecta pelo ID do círculo para qual versão da aplicação uma determinada requisição deve ser encaminhada. Vejamos o exemplo abaixo:

![](../.gitbook/assets/como_integrar_circulos_com_servicos_copy%20%281%29.png)

Na prática, em algum momento durante a interação do usuário com a sua aplicação \(**`App1`**\) - por exemplo, o login - o serviço **`Identify`** do **`circle-matcher`** deverá ser acionado para obter o círculo.

Com isso, o ID deve ser repassado como valor no parâmetro **`x-circle-id`** localizado no header de todas as próximas chamadas dos seus serviços \(**`App2`**\). O Charles é responsável por propagar essa informação porque quando recebida no Kubernetes, será utilizada para redirecionar a requisição para a versão correspondente à release associada ao círculo.

Caso o **`x-circle-id`** não seja repassado, todas as requisições serão redirecionadas para versões **Default**, ou seja, para releases padrões das suas aplicações sem uma segmentação específica.

### **Mescla de serviços com versões diferentes na minha release**

Para facilitar o entendimento, vamos exemplificar com um cenário onde o seu ambiente possui dois serviços: **Aplicação A** e **Aplicação B** e os seus círculos devem fazer o uso das seguintes versões:

![](../.gitbook/assets/versoes_diferentes_na_minha_release%20%281%29%20%281%29.png)

Sendo assim, a lógica de redirecionamento utilizando o **`x-circle-id`**será:

1. O usuário envia no header: `x-circle-id="Círculo QA"`. Nesse círculo, a chamada será redirecionada para a **versão X** do serviço **Aplicação A** e a **versão Y** do serviço **Aplicação B**. 
2. O usuário envia no header: `x-circle-id=”Circulo Dev”`. Nesse círculo, a chamada será redirecionada para a **versão Z** do serviço **Aplicação A e a versão Z** do serviço **Aplicação B.**

![](../.gitbook/assets/versoes_diferentes_na_minha_release_ii-1-%20%281%29.png)

## Como rotear círculos com cluster de Kubernetes?

O **Charles** envolve o [**Kubernetes**](https://kubernetes.io/docs/home/) ****e o ****[**Istio**](https://istio.io/docs/) ****no roteamento de tráfego, considere o seguinte cenário onde existe dois círculos:

* Moradores de Campinas \(identificado pelo ID 1234\);
* Moradores de Belo Horizonte \(identificado pelo ID 8746\).

Em ambos os círculos foram implantadas releases do serviço chamado "**application**", mas com versões diferentes:

* Moradores de Campinas \(1234\): utiliza a versão v2.
* Moradores de Belo Horizonte \(8746\): utiliza a versão v3.

Além disso, existe uma versão default \(v1\) para usuários que não se encaixam em algum círculo específico.

Suponha que, ao realizar a requisição para identificação do usuário, seja retornado o id 8756. Com isso, essa informação deverá ser repassada nas próximas interações com serviços através do header `x-circle-id`. A imagem abaixo retrata como o Charles utiliza internamente os recursos para rotear a release correta:

![](../.gitbook/assets/cluster_de_kubernetes%20%281%29%20%281%29.png)

Ao realizar a implantação de uma versão em um círculo, o Charles realiza todas as configurações para que o roteamento seja feito da maneira correta. Para entender melhor como ele acontece, vamos utilizar um cenário onde uma requisição vem de um serviço fora da stack, como mostra na figura acima.

A requisição será recebida pela Ingress, que realiza o controle do tráfego para a malha de serviços.

1. Uma vez permitida a entrada da requisição, o Virtual Service consulta o conjunto de regras de roteamento de tráfego a serem aplicadas no host endereçado. Nesse caso, a avaliação acontece através da especificação do header `x-circle-id`de maneira que o tráfego corresponda ao serviço "**application**".  
2. Além do serviço, também é necessário saber qual subconjunto definido no registro. Essa verificação é feita no \_\_**Destination Rules.**  
3. O redirecionamento do tráfego é realizado com base nas informações anteriores, chegando então à versão do serviço.   
4. Caso o `x-circle-id` não seja informado, existe uma regra definida no _Virtual Service_ que irá encaminhar para a versão padrão \(v1\).

