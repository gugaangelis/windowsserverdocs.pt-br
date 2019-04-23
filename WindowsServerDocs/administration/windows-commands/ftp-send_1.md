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
ms.openlocfilehash: 3dca11f0d534eb875a71fa2c39cdd4dc674ad788
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862117"
---
# <a name="ftp-send1"></a>ftp: send_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
send <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|<LocalFile>|Especifica o arquivo local para copiar.|  
|<remoteFile>|Especifica o nome a ser usado no computador remoto.|  
## <a name="remarks"></a>Comentários  
-   O **envie** comando é idêntico de **colocar** comando.  
-   Se *Arquivo_remoto* não for especificado, o arquivo é fornecido o *LocalFile* nome.  
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
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
