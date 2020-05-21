---
title: pnputil
description: Saiba como gerenciar o repositório de drivers com o utilitário pnputil. exe.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4c6bcb138e8bd7308c01c2c53fba83b69362298a
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436361"
---
# <a name="pnputil"></a>pnputil

O pnputil. exe é um utilitário de linha de comando que você pode usar para gerenciar o repositório de drivers. Você pode usar o PnPUtil para adicionar pacotes de driver, remover pacotes de driver e listar pacotes de driver que estão na loja.

## <a name="syntax"></a>Sintaxe

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-a|Especifica a adição do arquivo INF identificado.|
|-d|Especifica para excluir o arquivo INF identificado.|
|-E|Especifica a enumeração de todos os arquivos INF de terceiros.|
|-f|Especifica a força da exclusão do arquivo INF identificado. Não pode ser usado em conjunto com o parâmetro **– i** .|
|-i|Especifica a instalação do arquivo INF identificado. Não pode ser usado em conjunto com o parâmetro **-f** .|
|/?|Exibe a ajuda no prompt de comando.|


## <a name="examples"></a>Exemplos

-   pnputil. exe-um a:\usbcam\USBCAM. INF adiciona o arquivo INF que é especificado por USBCAM. INI
-   pnputil. exe-um c:\drivers \* . inf adiciona todos os arquivos INF em c:\drivers\
-   pnputil. exe-i-a a:\usbcam\USBCAM. INF adiciona e instala o driver especificado.
-   pnputil. exe – e enumera todos os drivers de terceiros.
-   pnputil. exe-d Oem0. inf exclui o especificado.
-   pnputil. exe-f-d Oem0. inf força a exclusão do arquivo INF especificado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Popd](popd.md)