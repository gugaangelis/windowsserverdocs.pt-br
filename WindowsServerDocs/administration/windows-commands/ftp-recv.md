---
title: recebimento de FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ece259f2d48e18f6a789d51b1df7089490f2fa1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725123"
---
# <a name="ftp-recv"></a>FTP: recv

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
  ## <a name="examples"></a>Exemplos  
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
