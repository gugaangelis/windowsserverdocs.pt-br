---
title: Adicionar um iFrame para uma extensão de ferramenta
description: Desenvolver uma extensão da ferramenta (projeto Paulo) do SDK do Windows Admin Center – adicionar um iFrame para uma extensão de ferramenta
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: da3850b75a0e069f9153d3c66baef9f00b67d61c
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452593"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>Adicionar um iFrame para uma extensão de ferramenta

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Neste artigo, adicionaremos um iFrame para uma extensão de ferramenta nova e vazia que criamos com a CLI do Windows Admin Center.

## <a name="prepare-your-environment"></a>Prepare o ambiente ##

Se você ainda não fez isso, siga as instruções em [desenvolver uma extensão da ferramenta](../develop-tool.md) para preparar seu ambiente e criar um novo, vazio extensão da ferramenta.

## <a name="add-a-module-to-your-project"></a>Adicionar um módulo ao seu projeto ##

Adicione um novo [módulo vazio](add-module.md) ao seu projeto ao qual adicionaremos um iFrame na próxima etapa.  

## <a name="add-an-iframe-to-your-module"></a>Adicionar um iFrame ao módulo ##

Agora, adicionaremos um iFrame para esse módulo novo e vazio que acabamos de criar.

No \src\app\, procurar em sua pasta de módulo e, em seguida, abrir o arquivo ```{!module-name}.component.html```, encontrado com a seguinte convenção de nomenclatura:

| Valor | Explicação | Exemplo de nome de arquivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Seu nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal.component.html``` |
    
Adicione o seguinte conteúdo para o arquivo html:

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

Isso é tudo, você adicionou um iFrame para a sua extensão.  Em seguida, você pode [compilação e do lado do carregamento](../develop-tool.md#build-and-side-load-your-extension) sua extensão no Windows Admin Center para ver os resultados.

> [!Note]
> Configurações de política de segurança (CSP) de conteúdo pode impedir que alguns sites renderização em um iFrame dentro do Windows Admin Center. Você pode aprender mais sobre isso [aqui](https://content-security-policy.com/). 
