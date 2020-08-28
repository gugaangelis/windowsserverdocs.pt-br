---
title: sxstrace
description: Saiba como diagnosticar problemas lado a lado.
ms.topic: reference
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a5398a9dcab96719de998a86bfa74df67a3f39b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024590"
---
# <a name="sxstrace"></a>sxstrace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diagnostica problemas lado a lado.

## <a name="syntax"></a>Sintaxe
```
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]
```

#### <a name="parameters"></a>Parâmetros
|Parâmetro|DESCRIÇÃO|
|-------|--------|
|rastreamento|Habilita o rastreamento para SxS (lado a lado)|
|-Logfile|Especifica o arquivo de log bruto.|
|\<FileName>|Salva o log de rastreamento em *filename*.|
|-nostop|Especifica que não há prompt para interromper o rastreamento.|
|analisar|Traduz o arquivo de rastreamento bruto.|
|-outfile|Especifica o nome de arquivo de saída.|
|\<ParsedFile>|Especifica o nome do arquivo analisado.|
|-filter|Permite que a saída seja filtrada.|
|\<AppName>|Especifica o nome do aplicativo.|
|stoptrace|Pare o rastreamento se ele não for interrompido antes.|
|-?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a>Exemplos
Habilite o rastreamento e salve o arquivo de rastreamento em **sxstrace. etl**:
```
sxstrace trace -logfile:sxstrace.etl
```
Traduza o arquivo de rastreamento bruto em um formato legível por humanos e salve o resultado em **sxstrace.txt**:
```
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt
```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

