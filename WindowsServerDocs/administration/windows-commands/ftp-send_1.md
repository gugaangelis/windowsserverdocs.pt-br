---
title: send_1 FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 04617246c05edde127db01ce1a0fe692eb0aceb1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725104"
---
# <a name="ftp-send_1"></a>FTP: send_1

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
send <LocalFile> [<remoteFile>]  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                    Descrição                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         Especifica o arquivo local a ser copiado.         |
| <remoteFile> | Especifica o nome a ser usado no computador remoto. |

## <a name="remarks"></a>Comentários  
- O comando **Send** é idêntico ao comando **Put** .  
- Se *remotefile* não for especificado, o arquivo receberá o nome de *LocalFile* .  
  ## <a name="examples"></a>Exemplos  
  Copie o arquivo local **Test. txt** e nomeie-o **Test1. txt** no computador remoto.  
  ```  
  send test.txt test1.txt  
  ```  
  Copie o arquivo local **Program. exe** para o computador remoto.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
