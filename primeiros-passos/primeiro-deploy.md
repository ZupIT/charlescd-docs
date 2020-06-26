---
description: >-
  Nesta seção, você encontra os passos para realizar o seu primeiro deploy
  utilizando o Charles.
---

# Primeiro Deploy

{% hint style="info" %}
Após criar o seu primeiro [**módulo** ](criando-modulos.md)e cadastrar as [**credenciais do seu cluster**,](definindo-workspace/configurando-cluster.md) você completou todos os passos de configuração necessários para a realização do seu primeiro deploy. Agora, é necessário criar uma [**release**](../referencia/release.md) ****e disponibilizá-la no cluster configurado.
{% endhint %}

No CharlesCD, oferecemos duas alternativas para a criação de uma release: utilizar o fluxo de um quadro de [**hipótese**](../referencia/hipotese.md) ****ou criá-la a partir de imagens docker já disponíveis no [**registry** ](definindo-workspace/docker-registry.md)configurado. 

Nesta seção de passos iremos focar a primeira abordagem:

1. Clique na opção **Hypotheses** que aparece na barra lateral esquerda na página inicial;
2.  Preencha o nome da nova hipótese a ser criada no campo **Create new hypothesis** na parte inferior da listagem de hipóteses e aperte a **tecla enter**;
3. No quadro da hipótese recém criada, clique em **+ Card** na parte inferior da coluna **To Do**;
4. Digite o nome do seu novo card e aperte a **tecla enter**;
5. Clique com o botão esquerdo no card e associe-o ao **Módulo** criado anteriormente. **A partir deste momento uma branch com o nome do card será criada no repositório configurado**;
6. Realize seu trabalho normalmente nesta branch e assim que finalizar, mova o card para a coluna **Ready to Go**;
7. Clique em **Generate release candidate** na parte inferior da coluna **Ready to Go**;
8. Digite o nome da release a ser criada e clique em **Generate**. Neste momento uma branch com o prefixo "**release-darwin" será criada no repositório do módulo, disparando assim a ferramenta de CI configurada**;
9. Um card com o status **Building** aparecerá na coluna **Builds**. Aguarde até que o status dele passe para **Built.**

Depois que você realizou o processo acima, sua release está pronta para o deploy. No exemplo abaixo, é a release em destaque: 

![](../.gitbook/assets/chrome-capture-4-.gif)

Agora siga os seguintes passos para o deploy em[ **mar aberto:**](../principais-conceitos.md#mar-aberto-default)\*\*\*\*

1. Na tela inicial do Charles, clique em **Circles**;
2. Clique no círculo **Default** \(Este representa o mar aberto\);
3. Clique em **Override release** no canto direito superior;
4. Clique em **Search for ready releases**;
5. Digite o nome da release criada nos passos acima, a selecione e clique em **Deploy.**

Depois disso, o Charles se encarregará de disponibilizar a release criada no cluster configurado em mar aberto. O status do deploy será exibido e atualizado conforme o progresso.

![Exemplo de deploy em mar aberto](../.gitbook/assets/primeiro-deploy.gif)

