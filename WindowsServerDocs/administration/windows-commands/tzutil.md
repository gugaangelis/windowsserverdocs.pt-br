---
title: tzutil
description: Artigo de referência para tzutil, que exibe o utilitário de fuso horário do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99d88057c88a55aaf529d238088f8422c33e9ba7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937291"
---
# <a name="tzutil"></a>tzutil

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe o utilitário de fuso horário do Windows.

## <a name="syntax"></a>Sintaxe
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/?|Exibe a ajuda no prompt de comando.|
|/g|Exibe a ID do fuso horário atual.|
|/s \<timeZoneID> [_dstoff]|Define o fuso horário atual usando a ID de fuso horário especificada. O sufixo **_dstoff** desabilita os ajustes de horário de verão para o fuso horário (quando aplicável).|
|/l|lista todas as IDs de fuso horário e nomes de exibição válidos. A saída será:<p>-   \<display name><br />-   \<time zone ID>|

## <a name="remarks"></a>Comentários
Um código de saída **0** indica que o comando foi concluído com êxito.

## <a name="examples"></a>Exemplos
Para exibir a ID do fuso horário atual, digite:
```
tzutil /g
```
Para definir o fuso horário atual como hora padrão do Pacífico, digite:
```
tzutil /s Pacific Standard time
```
Para definir o fuso horário atual como hora padrão do Pacífico e desabilitar os ajustes de horário de verão, digite:
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

