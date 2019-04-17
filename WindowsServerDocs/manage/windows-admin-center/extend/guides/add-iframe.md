---
title: Adicionar um iFrame para uma extensão de ferramenta
description: Desenvolver uma extensão de ferramenta SDK do Windows Admin Center (Project Honolulu) - adicionar um iFrame para uma extensão de ferramenta
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b4a7b688e4b2d9f52e44395c19211b91b964578
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080923"
---
# Adicionar um iFrame para uma extensão de ferramenta

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

Neste artigo, vamos adicionar um iFrame como uma extensão de ferramenta nova e vazia que criamos com a CLI do Windows Admin Center.

## Preparar o ambiente ##

Se você ainda não fez, siga as instruções em [desenvolver uma extensão de ferramenta](..\develop-tool.md) para preparar seu ambiente e criar uma extensão de ferramenta nova e vazia.

## Adicionar um módulo ao seu projeto ##

Adicione um novo [módulo vazio](add-module.md) ao seu projeto, ao qual adicionaremos um iFrame na próxima etapa.  

## Adicionar um iFrame para seu módulo ##

Agora vamos adicionar um iFrame para esse módulo novo e vazio que acabamos de criar.

Em \src\app\, navegue para a pasta de módulo e abra o arquivo ```{!module-name}.component.html```, encontrado com a seguinte convenção de nomenclatura:

| Valor | Explicação | Exemplo de nome de arquivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | O nome do módulo (letras minúsculas, espaços substituídos por traços) | ```manage-foo-works-portal.component.html``` |
    
Adicione o seguinte conteúdo para o arquivo html:

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

Isso é tudo, você adicionou um iFrame até sua extensão.  Em seguida, você pode [carga de compilação e lado](..\develop-tool.md#build-and-side-load-your-extension) sua extensão no Centro de administração do Windows para ver os resultados.
