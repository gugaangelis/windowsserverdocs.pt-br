---
title: recebimento de FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ec35a2044945e3d39a2a78d39923de3a56eb18d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376122"
---
# <a name="ftp-recv"></a>FTP: recv

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo remoto para o computador local usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
recv <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parâmetros  

|   Parâmetro   |                   Descrição                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Especifica o arquivo remoto a ser copiado.        |
| [<LocalFile>] | Especifica o nome a ser usado no computador local. |

## <a name="remarks"></a>Comentários  
- O comando **recv** é idêntico ao comando **Get** .  
- Se *LocalFile* não for especificado, o arquivo receberá o nome *remotefile* .  
  ## <a name="BKMK_Examples"></a>Disso  
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
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
