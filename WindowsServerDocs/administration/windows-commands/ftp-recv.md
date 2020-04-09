---
title: recebimento de FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fec409741e00bb3e6f61808630e5141ce4ec78f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842959"
---
# <a name="ftp-recv"></a>FTP: recv

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo remoto para o computador local usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
recv <remoteFile> [<LocalFile>]  
```  
#### <a name="parameters"></a>Parâmetros  

|   Parâmetro   |                   Descrição                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Especifica o arquivo remoto a ser copiado.        |
| [<LocalFile>] | Especifica o nome a ser usado no computador local. |

## <a name="remarks"></a>Comentários  
- O comando **recv** é idêntico ao comando **Get** .  
- Se *LocalFile* não for especificado, o arquivo receberá o nome *remotefile* .  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
  Copie **Test. txt** para o computador local usando o tipo de transferência de arquivo atual.  
  ```  
  recv test.txt  
  ```  
  Copie **Test. txt** para o computador local como **Test1. txt** usando o tipo de transferência de arquivo atual.  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [FTP: ASCII](ftp-ascii.md)  
- [FTP: binário](ftp-binary.md)  
- [FTP: obter](ftp-get.md)  
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
