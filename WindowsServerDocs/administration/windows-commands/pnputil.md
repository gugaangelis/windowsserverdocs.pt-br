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
ms.openlocfilehash: 134e6ce4b1fc44450047de3287b7daac67da4b6a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837509"
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
|{1&gt;-e&lt;1}|Especifica a enumeração de todos os arquivos INF de terceiros.|
|-f|Especifica a força da exclusão do arquivo INF identificado. Não pode ser usado em conjunto com o parâmetro **– i** .|
|-i|Especifica a instalação do arquivo INF identificado. Não pode ser usado em conjunto com o parâmetro **-f** .|
|/?|Exibe a ajuda no prompt de comando.|


## <a name="examples"></a>Exemplos

-   pnputil. exe-um a:\usbcam\USBCAM. INF adiciona o arquivo INF que é especificado por USBCAM. INI
-   pnputil. exe-um c:\drivers\*. inf adiciona todos os arquivos INF em c:\drivers\
-   pnputil. exe-i-a a:\usbcam\USBCAM. INF adiciona e instala o driver especificado.
-   pnputil. exe – e enumera todos os drivers de terceiros.
-   pnputil. exe-d Oem0. inf exclui o especificado.
-   pnputil. exe-f-d Oem0. inf força a exclusão do arquivo INF especificado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Popd](popd.md)