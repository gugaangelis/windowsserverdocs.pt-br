---
title: Get de FTP
description: Tópico de comandos do Windows para FTP Get
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0b4dc41ec29edfb94661176a5ccaf651584fc43
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843479"
---
# <a name="ftp-get"></a>FTP: obter

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo remoto para o computador local usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
get <remoteFile> [<LocalFile>]  
```  
#### <a name="parameters"></a>Parâmetros  

|   Parâmetro   |                                                              Descrição                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   Especifica o arquivo remoto a ser copiado.                                                   |
| [<LocalFile>] | Especifica o nome do arquivo a ser usado no computador local. Se *LocalFile* não for especificado, o arquivo receberá o nome *remotefile* . |

## <a name="remarks"></a>Comentários  
O comando **Get** é idêntico ao comando **recv** .  
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
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
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
