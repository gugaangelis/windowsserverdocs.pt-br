---
title: remotehelp_1 FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc4affb3f04eadaa4e0005e5edce0f564156f64a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725131"
---
# <a name="ftp-remotehelp_1"></a>FTP: remotehelp_1

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe a ajuda para comandos remotos.   
## <a name="syntax"></a>Sintaxe  
```  
remotehelp [<Command>]  
```  
#### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<Command>]|Especifica o nome do comando sobre o qual você deseja obter ajuda. Se o *comando* não for especificado, o **FTP** exibirá uma lista de todos os comandos remotos.|  
## <a name="remarks"></a>Comentários  
Você pode executar comandos remotos usando **aspas** ou **literais**.  
## <a name="examples"></a>Exemplos  
Exibir uma lista de comandos remotos.  
```  
remotehelp  
```  
Exiba a sintaxe para o **comando remoto feito** .  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: cotação](ftp-quote.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
