---
title: pnputil
description: Saiba como gerenciar o repositório de drivers com o utilitário pnputil. exe.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: f20c60bfd9ae33497dd356c7797b9fb1d2b51d18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372295"
---
# <a name="pnputil"></a>pnputil

O pnputil. exe é um utilitário de linha de comando que você pode usar para gerenciar o repositório de drivers. Você pode usar o PnPUtil para adicionar pacotes de driver, remover pacotes de driver e listar pacotes de driver que estão na loja.

## <a name="syntax"></a>Sintaxe

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-a|Especifica a adição do arquivo INF identificado.|
|-d|Especifica para excluir o arquivo INF identificado.|
|-e|Especifica a enumeração de todos os arquivos INF de terceiros.|
|-f|Especifica a força da exclusão do arquivo INF identificado. Não pode ser usado em conjunto com o parâmetro **– i** .|
|-i|Especifica a instalação do arquivo INF identificado. Não pode ser usado em conjunto com o parâmetro **-f** .|
|/?|Exibe a ajuda no prompt de comando.|


## <a name="examples"></a>Exemplos

-   pnputil. exe-um a:\usbcam\USBCAM. INF adiciona o arquivo INF que é especificado por USBCAM. INI
-   pnputil. exe-a c:\ drivers\*.inf adiciona todos os arquivos INF em c:\drivers\
-   pnputil. exe-i-a a:\usbcam\USBCAM. INF adiciona e instala o driver especificado.
-   pnputil. exe – e enumera todos os drivers de terceiros.
-   pnputil. exe-d Oem0. inf exclui o especificado.
-   pnputil. exe-f-d Oem0. inf força a exclusão do arquivo INF especificado.

## <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Popd](popd.md)