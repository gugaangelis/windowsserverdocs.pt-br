---
title: remotehelp_1 FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bac6fbe4a55c3fed4caab4e30ba848ec9ea68e21
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376032"
---
# <a name="ftp-remotehelp_1"></a>FTP: remotehelp_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe a ajuda para comandos remotos.   
## <a name="syntax"></a>Sintaxe  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<Command>]|Especifica o nome do comando sobre o qual você deseja obter ajuda. Se o *comando* não for especificado, o **FTP** exibirá uma lista de todos os comandos remotos.|  
## <a name="remarks"></a>Comentários  
Você pode executar comandos remotos usando **aspas** ou **literais**.  
## <a name="BKMK_Examples"></a>Disso  
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
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
