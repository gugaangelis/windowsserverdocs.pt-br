---
title: Get de FTP
description: Tópico de comandos do Windows para FTP Get
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cc74b56fa849a25ed2f4e4a37d339b1da87c24f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376394"
---
# <a name="ftp-get"></a>FTP: obter

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo remoto para o computador local usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parâmetros  

|   Parâmetro   |                                                              Descrição                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   Especifica o arquivo remoto a ser copiado.                                                   |
| [<LocalFile>] | Especifica o nome do arquivo a ser usado no computador local. Se *LocalFile* não for especificado, o arquivo receberá o nome *remotefile* . |

## <a name="remarks"></a>Comentários  
O comando **Get** é idêntico ao comando **recv** .  
## <a name="BKMK_Examples"></a>Disso  
Copie **Test. txt** para o computador local usando o tipo de transferência de arquivo atual.  
```  
get test.txt  
```  
Copie **Test. txt** para o computador local como **Test1. txt** usando o tipo de transferência de arquivo atual.  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binário](ftp-binary.md)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
