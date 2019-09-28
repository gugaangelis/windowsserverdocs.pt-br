---
title: sxstrace
description: Saiba como diagnosticar problemas lado a lado.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 66326943bf1b056951ae5824df5a4f60892492cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370715"
---
# <a name="sxstrace"></a>sxstrace

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diagnostica problemas lado a lado.    

## <a name="syntax"></a>Sintaxe  
```  
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]  
```  

### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|Rastreou|Habilita o rastreamento para SxS (lado a lado)|  
|-Logfile|Especifica o arquivo de log bruto.|  
|\<Nome de arquivo >|Salva o log de rastreamento em *filename*.|  
|-nostop|Especifica que não há prompt para interromper o rastreamento.|  
|Passar|Traduz o arquivo de rastreamento bruto.|  
|-outfile|Especifica o nome de arquivo de saída.|  
|\<> ParsedFile|Especifica o nome do arquivo analisado.|  
|-filtro|Permite que a saída seja filtrada.|  
|\<AppName >|Especifica o nome do aplicativo.|  
|stoptrace|Pare o rastreamento se ele não for interrompido antes.|  
|-?|Exibe a ajuda no prompt de comando.|  

## <a name="BKMK_Examples"></a>Disso  
Habilite o rastreamento e salve o arquivo de rastreamento em **sxstrace. etl**:  
```  
sxstrace trace -logfile:sxstrace.etl  
```  
Traduza o arquivo de rastreamento bruto em um formato legível por humanos e salve o resultado em **sxstrace. txt**:  
```  
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
