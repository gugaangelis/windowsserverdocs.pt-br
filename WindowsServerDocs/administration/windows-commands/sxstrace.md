---
title: sxstrace
description: Saiba como diagnosticar problemas de lado a lado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 396d06bf079c0cfa8ba4864f71333eec39f7b255
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814097"
---
# <a name="sxstrace"></a>sxstrace

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diagnostica problemas de lado a lado.    

## <a name="syntax"></a>Sintaxe  
```  
sxstrace [{[trace /logfile:<FileName> [/nostop]|[parse /logfile:<FileName> /outfile:<ParsedFile>  [/filter:<AppName>]}]  
```  

### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|rastreamento|Habilitam o rastreamento de sxs (lado a lado)|  
|/logfile|Especifica o arquivo de log bruto.|  
|\<FileName>|Salva o log de rastreamento *FileName*.|  
|/nostop|Não especifica que nenhum prompt para interromper o rastreamento.|  
|Analisar|Converte o arquivo de rastreamento bruto.|  
|/outfile|Especifica o nome do arquivo de saída.|  
|\<ParsedFile>|Especifica o nome do arquivo do arquivo analisado.|  
|/filter|Permite que a saída a ser filtrada.|  
|\<AppName>|Especifica o nome do aplicativo.|  
|stoptrace|Pare o rastreamento se ele não está parado antes.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="BKMK_Examples"></a>Exemplos  
Habilitar o rastreamento e salvar o arquivo de rastreamento para **sxstrace.etl**:  
```  
sxstrace trace /logfile:sxstrace.etl  
```  
Converter o arquivo de rastreamento bruto em um formato legível e salvar o resultado para **sxstrace.txt**:  
```  
sxstrace parse /logfile:sxstrace.etl /outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  
