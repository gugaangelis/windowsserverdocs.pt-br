---
title: send_1 FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd9f658b4fbfa5f6c9fa9a58fb0c524ad53627bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376004"
---
# <a name="ftp-send_1"></a>FTP: send_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
send <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                    Descrição                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         Especifica o arquivo local a ser copiado.         |
| <remoteFile> | Especifica o nome a ser usado no computador remoto. |

## <a name="remarks"></a>Comentários  
- O comando **Send** é idêntico ao comando **Put** .  
- Se *remotefile* não for especificado, o arquivo receberá o nome de *LocalFile* .  
  ## <a name="BKMK_Examples"></a>Disso  
  Copie o arquivo local **Test. txt** e nomeie-o **Test1. txt** no computador remoto.  
  ```  
  send test.txt test1.txt  
  ```  
  Copie o arquivo local **Program. exe** para o computador remoto.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
