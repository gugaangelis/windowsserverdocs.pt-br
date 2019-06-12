---
title: send_1 de FTP
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 39423aff3c64f41c4fc0f8998484e6dcc38f822e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438444"
---
# <a name="ftp-send1"></a>ftp: send_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
send <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                    Descrição                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         Especifica o arquivo local para copiar.         |
| <remoteFile> | Especifica o nome a ser usado no computador remoto. |

## <a name="remarks"></a>Comentários  
- O **envie** comando é idêntico de **colocar** comando.  
- Se *Arquivo_remoto* não for especificado, o arquivo é fornecido o *LocalFile* nome.  
  ## <a name="BKMK_Examples"></a>Exemplos  
  Copie o arquivo local **Test. txt** e nomeie-o **test1.txt** no computador remoto.  
  ```  
  send test.txt test1.txt  
  ```  
  Copie o arquivo local **program.exe** ao computador remoto.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
