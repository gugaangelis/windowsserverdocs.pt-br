---
title: tzutil
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41a46ea7974b67cc557973484428480e7beb5484
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876797"
---
# <a name="tzutil"></a>tzutil

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe o Windows hora em que o utilitário de zona. 
## <a name="syntax"></a>Sintaxe
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/?|Exibe a ajuda no prompt de comando.|
|/g|Exibe a ID de fuso horário atual.|
|/s \<timeZoneID>[_dstoff]|Define o fuso horário atual usando a ID de fuso horário especificado. O **_dstoff** sufixo desabilita os ajustes do horário de verão para o fuso horário (quando aplicável).|
|/l|listas de hora válida de todas as IDs de zona e nomes de exibição. A saída será:<br /><br />-   \<display name><br />-   \<ID do fuso horário >|

## <a name="remarks"></a>Comentários
Um código de saída **0** indica o comando foi concluído com êxito.

## <a name="BKMK_Examples"></a>Exemplos
Para exibir a ID de fuso horário atual, digite:
```
tzutil /g
```
Para definir o fuso horário atual para a hora oficial do Pacífico, digite:
```
tzutil /s Pacific Standard time
```
Para definir o fuso horário atual para a hora oficial do Pacífico e desabilitar os ajustes de horário de verão, digite:
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)

