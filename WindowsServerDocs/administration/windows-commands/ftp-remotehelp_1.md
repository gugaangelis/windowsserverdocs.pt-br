---
title: ftp remotehelp_1
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd64af157f7ce05330cdafe6e4db6787fa765859
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889587"
---
# <a name="ftp-remotehelp1"></a>ftp: remotehelp_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe a Ajuda para comandos remotos.   
## <a name="syntax"></a>Sintaxe  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<Command>]|Especifica o nome do comando sobre o qual você deseja obter ajuda. Se *comando* não for especificado, **ftp** exibe uma lista de todos os comandos remotos.|  
## <a name="remarks"></a>Comentários  
Você pode executar comandos remotos usando **cotação** ou **literal**.  
## <a name="BKMK_Examples"></a>Exemplos  
Exiba uma lista de comandos remotos.  
```  
remotehelp  
```  
Exibir a sintaxe para o **feito** comando remoto.  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: quote](ftp-quote.md)  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
