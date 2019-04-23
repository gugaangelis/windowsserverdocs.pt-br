---
title: pnputil
description: Saiba como gerenciar o driver store com o utilitário pnputil.exe.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5bde78d97be8def9f8594572869c34ef213db480
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862537"
---
# <a name="pnputil"></a>pnputil

Pnputil.exe é um utilitário de linha de comando que você pode usar para gerenciar o armazenamento de driver. Você pode usar o Pnputil para adicionar pacotes de driver, remova pacotes de driver e pacotes de driver de lista que estão no repositório.

## <a name="syntax"></a>Sintaxe

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-a|Especifica para adicionar o arquivo INF identificado.|
|-d|Especifica para excluir o arquivo INF identificado.|
|-e|Especifica para enumerar todos os arquivos INF de terceiros.|
|-f|Especifica para forçar a exclusão do arquivo INF identificado. Não pode ser usado em conjunto com o **– i** parâmetro.|
|-i|Especifica para instalar o arquivo INF identificado. Não pode ser usado em conjunto com o **-f** parâmetro.|
|/?|Exibe a ajuda no prompt de comando.|


## <a name="examples"></a>Exemplos

-   pnputil.exe - a a:\usbcam\USBCAM. INF adiciona o arquivo INF que é especificado pelo USBCAM. INF
-   -a c:\drivers pnputil.exe\*. inf adiciona todos os arquivos INF em c:\drivers\
-   pnputil.exe -i - a a:\usbcam\USBCAM. INF adiciona e instala o driver especificado.
-   pnputil.exe – e enumera todos os drivers de terceiros.
-   Oem0 -d do pnputil.exe exclui especificado.
-   pnputil.exe -f -d oem0 força a exclusão do arquivo INF especificado.

## <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[Popd](popd.md)