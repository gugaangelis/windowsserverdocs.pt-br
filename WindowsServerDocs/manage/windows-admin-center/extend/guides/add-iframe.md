---
title: Adicionar um iFrame para uma extensão de ferramenta
description: Desenvolver uma extensão de ferramenta SDK do centro de administração do Windows (projeto Honolulu) – adicionar um iFrame a uma extensão de ferramenta
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0833b2fd92f2bf4b512120783bb71295a3112745
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406894"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>Adicionar um iFrame para uma extensão de ferramenta

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Neste artigo, adicionaremos um iFrame a uma nova extensão de ferramenta vazia que criamos com a CLI do centro de administração do Windows.

## <a name="prepare-your-environment"></a>Prepare o ambiente ##

Se você ainda não fez isso, siga as instruções em [desenvolver uma extensão de ferramenta](../develop-tool.md) para preparar seu ambiente e criar uma nova extensão de ferramenta vazia.

## <a name="add-a-module-to-your-project"></a>Adicionar um módulo ao seu projeto ##

Adicione um novo [módulo vazio](add-module.md) ao seu projeto, ao qual adicionaremos um iframe na próxima etapa.  

## <a name="add-an-iframe-to-your-module"></a>Adicionar um iFrame ao seu módulo ##

Agora, adicionaremos um iFrame a esse novo módulo vazio que acabamos de criar.

Em \src\app @ no__t-0, navegue até a pasta do módulo e, em seguida, abra o arquivo ```{!module-name}.component.html```, encontrado com a seguinte convenção de nomenclatura:

| Valor | Explicação | Exemplo de nome de arquivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Seu nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal.component.html``` |
    
Adicione o seguinte conteúdo ao arquivo HTML:

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

É isso, você adicionou um iFrame à sua extensão.  Em seguida, você pode [criar e carregar o lado](../develop-tool.md#build-and-side-load-your-extension) de sua extensão no centro de administração do Windows para ver os resultados.

> [!Note]
> As configurações de CSP (política de segurança de conteúdo) podem impedir que alguns sites sejam renderizados em um iFrame no centro de administração do Windows. Você pode aprender mais sobre isso [aqui](https://content-security-policy.com/). 
