# Release

Releases são as versões de uma aplicação. Diferente de outras formas de deploy, em que as releases geralmente passam por diversos ambientes até chegar ao de produção, no CharlesCD é possível que uma mesma release seja publicada para diferentes [**círculos**](circulos.md)**.**

## Como criar releases pelo Charles?

Existem **duas maneiras de criar releases** no Charles. São elas:

1. Por meio de quadro de hipóteses;
2. Por imagens existentes no docker registry.

A primeira delas oferece todo o potencial de uso do produto, pois trabalha com o conceito de teste de hipóteses durante todo o ciclo de desenvolvimento. Já a segunda oferece a flexibilidade necessária para casos em que é desejado que toda a parte de desenvolvimento e geração de artefatos estejam apartadas do CharlesCD.

### Release por meio do quadro de hipóteses

Após o cadastro de uma [**hipótese**](hipotese.md) dentro do Charles, você pode utilizar o quadro que é gerado automaticamente para criar e gerenciar cartões que representam o desenvolvimento da sua hipótese.

Neste quadro, temos duas categorias de cartões: o **azul que representa a codificação de uma feature** e o **cinza que caracteriza às ações que não envolvem implementação**.

Para gerar novas releases, os cartões azuis, que representam as features, são os que importam. Quando eles estão na coluna **Ready to Go**, você seleciona apenas um ou um conjunto deles para construir a release.

![Exemplo de sele&#xE7;&#xE3;o de cards](../.gitbook/assets/gerando-release-board-1-%20%281%29%20%281%29.gif)

Assim que a criação de uma release é acionada, uma branch com o prefixo **release-darwin** será criada no **repositório do módulo, disparando a ferramenta de CI configurada**. Além disso, um novo cartão com o estado "**Building**" aparecerá na coluna **Builds** \_\_para representar o processo em andamento.

{% hint style="warning" %}
Ao acionar o pipeline da sua ferramenta de CI através do prefixo **release-darwin**, é esperado que ela gere uma imagem da sua aplicação e faça o push para o seu [**registry**](../primeiros-passos/definindo-workspace/docker-registry.md).
{% endhint %}

A partir desse momento, o [**Villager**](https://github.com/ZupIT/charlescd/tree/master/villager) observará o seu registry em busca da release gerada. Aguarde até que o estado do cartão passe para _Built_.

{% hint style="info" %}
Qualquer caso de sucesso ou erro será mostrado no estado do cartão da release.
{% endhint %}

![Exemplo de como aparecem os status das releases](../.gitbook/assets/builts%20%281%29.png)

### **Releases por meio de imagens existentes no Docker Registry configurado**

Para criar uma release sem passar pelo quadro de hipóteses, é preciso que as imagens no Docker já estejam disponíveis no [**registry configurado**](../primeiros-passos/definindo-workspace/docker-registry.md) para o módulo. Se esse requisito já estiver feito, basta clicar na opção **Circles** no menu do Charles e selecionar o círculo desejado para o deploy da release a ser criada.

Caso o círculo esteja sendo criado neste momento, clique em **Insert release** e logo após em **Create release**. Se o círculo já existir, clique em **Override release** e depois em **Create release.**

Na tela de criação de releases, preencha o nome e selecione um módulo e sua componente. No campo ao lado, todas as imagens disponíveis daquela componente serão listadas no registry. Selecione uma e se for necessário adicione mais módulos a release, clique em **Add modules** e repita o processo anterior. Quando todos os seus módulos estiverem cadastrados, clique em **deploy**.

![Exemplo de cria&#xE7;&#xE3;o de release por imagens do Registry](../.gitbook/assets/releases-por-meio-de-imagens-existentes%20%282%29.gif)

Após o deploy desta nova release, ela estará disponível para utilização em outros círculos a partir da opção **"Search for existing releases".**

## Como buscar uma release existente?

Se a release foi gerada através do [**quadro de hipóteses**](hipotese.md#gestao-do-board) no seu workspace, ao realizar o deploy em um círculo, você pode buscá-la através da opção: **"**_Search for existing releases_".

![Exemplo de pesquisa de release pelo deploy no c&#xED;rculo](../.gitbook/assets/may-29-2020_17-21-33%20%282%29.gif)

