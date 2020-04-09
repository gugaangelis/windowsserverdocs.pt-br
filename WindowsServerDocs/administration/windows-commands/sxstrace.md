---
title: sxstrace
description: Saiba como diagnosticar problemas lado a lado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ece727b68eb620e839cbfb8efe02dbe775666498
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833609"
---
# <a name="sxstrace"></a>sxstrace

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diagnostica problemas lado a lado.    

## <a name="syntax"></a>Sintaxe  
```  
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]  
```  

#### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|trace|Habilita o rastreamento para SxS (lado a lado)|  
|-Logfile|Especifica o arquivo de log bruto.|  
|\<nome de arquivo >|Salva o log de rastreamento em *filename*.|  
|-nostop|Especifica que não há prompt para interromper o rastreamento.|  
|analisar|Traduz o arquivo de rastreamento bruto.|  
|-outfile|Especifica o nome de arquivo de saída.|  
|\<ParsedFile >|Especifica o nome do arquivo analisado.|  
|-filtro|Permite que a saída seja filtrada.|  
|\<AppName >|Especifica o nome do aplicativo.|  
|stoptrace|Pare o rastreamento se ele não for interrompido antes.|  
|-?|Exibe a ajuda no prompt de comando.|  

## <a name="examples"></a><a name="BKMK_Examples"></a>Disso  
Habilite o rastreamento e salve o arquivo de rastreamento em **sxstrace. etl**:  
```  
sxstrace trace -logfile:sxstrace.etl  
```  
Traduza o arquivo de rastreamento bruto em um formato legível por humanos e salve o resultado em **sxstrace. txt**:  
```  
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
