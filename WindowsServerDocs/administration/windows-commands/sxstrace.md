---
title: sxstrace
description: Artigo de referência para o comando systrace, que ajuda a diagnosticar problemas lado a lado.
ms.topic: reference
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 95f52993db56e61b3a1cd4d26a0ef36626421a15
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718203"
---
# <a name="sxstrace"></a>sxstrace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diagnostica problemas lado a lado.

## <a name="syntax"></a>Sintaxe

```
sxstrace [{[trace -logfile:<filename> [-nostop]|[parse -logfile:<filename> -outfile:<parsedfile>  [-filter:<appname>]}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | DESCRIÇÃO |
|--|--|
| rastreamento | Habilita o rastreamento lado a lado. |
| -Logfile | Especifica o arquivo de log bruto. |
| `<filename>` | Salva o log de rastreamento no `<filename` . |
| -nostop | Especifica que você não deve receber uma solicitação para interromper o rastreamento. |
| analisar | Traduz o arquivo de rastreamento bruto. |
| -outfile | Especifica o nome de arquivo de saída. |
| `<parsedfile>` | Especifica o nome do arquivo analisado. |
| -filter | Permite que a saída seja filtrada. |
| `<appname>` | Especifica o nome do aplicativo. |
| stoptrace | Interrompe o rastreamento, se não tiver sido interrompido antes. |
| -? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para habilitar o rastreamento e salvar o arquivo de rastreamento em *sxstrace. etl*, digite:

```
sxstrace trace -logfile:sxstrace.etl
```

Para converter o arquivo de rastreamento bruto em um formato legível por humanos e salvar o resultado em *sxstrace.txt*, digite:

```
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
