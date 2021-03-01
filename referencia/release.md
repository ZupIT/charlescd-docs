---
description: >-
  Nesta seção, você encontra detalhes sobre como funcionam as releases no
  Charles.
---

# Release

Releases são as versões de uma aplicação. Diferente de outras formas de deploy, em que as releases geralmente passam por diversos ambientes até chegar ao de produção, no CharlesCD é possível que uma mesma release seja publicada para diferentes [**círculos**](circulo.md)**.**

## Como criar releases pelo Charles?

Veja como **criar releases** no Charles:

1. Por meio de imagens existentes no docker registry.

 Essa forma oferece a flexibilidade necessária para casos em que é desejado que toda a parte de desenvolvimento e geração de artefatos estejam apartadas do CharlesCD.

### **Releases por meio de imagens existentes no Docker Registry configurado**

Para criar uma release, é preciso que as imagens no Docker já estejam disponíveis no [**registry configurado**](../primeiros-passos/definindo-workspace/docker-registry.md) para o módulo. Se esse requisito já estiver feito, basta clicar na opção **Circles** no menu do Charles e selecionar o círculo desejado para o deploy da release a ser criada.

Caso o círculo esteja sendo criado neste momento, clique em **Insert release** e logo após em **Create release**. Se o círculo já existir, clique em **Override release** e depois em **Create release.**

Na tela de criação de releases, preencha o nome e selecione um módulo e sua componente. No campo ao lado, todas as imagens disponíveis daquela componente serão listadas no registry. Selecione uma e se for necessário adicione mais módulos a release, clique em **Add modules** e repita o processo anterior. Quando todos os seus módulos estiverem cadastrados, clique em **deploy**.

![Exemplo de cria&#xE7;&#xE3;o de release por imagens do Registry](../.gitbook/assets/releases-por-meio-de-imagens-existentes%20%282%29.gif)

Após o deploy desta nova release, ela estará disponível para utilização em outros círculos a partir da opção **"Search for existing releases".**



