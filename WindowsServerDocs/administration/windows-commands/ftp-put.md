---
title: Coloque de FTP
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3307ba71e7b3c8b4113f9ed29ab06660dafa5f6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868737"
---
# <a name="ftp-put"></a>FTP: put

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
put <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|<LocalFile>|Especifica o arquivo local para copiar.|  
|[<remoteFile>]|Especifica o nome a ser usado no computador remoto.|  
## <a name="remarks"></a>Comentários  
-   O **colocar** comando é idêntico de **enviar** comando.  
-   Se *Arquivo_remoto* não for especificado, o arquivo é fornecido o *LocalFile* nome.  
## <a name="BKMK_Examples"></a>Exemplos  
Copie o arquivo local **Test. txt** e nomeie-o **test1.txt** no computador remoto.  
```  
put test.txt test1.txt  
```  
Copie o arquivo local **program.exe** ao computador remoto.  
```  
put program.exe  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
