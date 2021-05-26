---
description: 'Nesta seção, você encontra detalhes de como configurar o Registry.'
---

# Registry

## Por que configurar o Registry?  <a id="why-do-you-have-to-configure-a-registry"></a>

‌Um dos pontos para o Charles trabalhar é saber onde estão as imagens da sua aplicação. Para fazer isso, é esperado que você armazene-as em um docker registry e garanta o acesso a elas. 

Uma vez que essa configuração foi feita, Charles está preparado para ler seu registro e fazer algumas ações, como: 

* Confirmar se a tag informada quando um estiver fazendo um deploy em um círculo é válida. 
* Usar essa permissão durante a geração de uma release por meio do quadro `villager` para procurar pela tag no registry e garantir que ela seja enviada e usada.

‌Charles já está integrado com alguns docker registries, escolha um e adicione as informações necessárias: 

{% page-ref page="azure-container-registry.md" %}

{% page-ref page="docker-hub.md" %}

{% page-ref page="ecr.md" %}

{% page-ref page="gcr.md" %}



