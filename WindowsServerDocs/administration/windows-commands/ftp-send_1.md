---
title: send_1 FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: df6ea9eda6fced91babca37639cd6efe981ba7de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842999"
---
# <a name="ftp-send_1"></a>FTP: send_1

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
  ## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
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
