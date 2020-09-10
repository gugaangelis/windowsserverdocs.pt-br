---
title: sxstrace
description: Saiba como diagnosticar problemas lado a lado.
ms.topic: reference
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7afcb79bf12cf23a66ac564362012c4424e5bbe6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626769"
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

