---
title: tzutil
description: Tópico de referência para tzutil, que exibe o utilitário de fuso horário do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4011f04b762522c8c0d157993bad71d88758d32f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721198"
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
|/s \<timezoneid> [_dstoff]|Define o fuso horário atual usando a ID de fuso horário especificada. O sufixo **_dstoff** desabilita os ajustes de horário de verão para o fuso horário (quando aplicável).|
|/l|lista todas as IDs de fuso horário e nomes de exibição válidos. A saída será:<p>-   \<> de nome de exibição<br />-   \<ID de fuso horário>|

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
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

