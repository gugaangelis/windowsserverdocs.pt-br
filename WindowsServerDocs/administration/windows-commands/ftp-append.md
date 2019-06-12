---
title: o acréscimo de FTP
description: 'O acréscimo de tópico de comandos do Windows para o ftp '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9580d725120bb32a9b915d37cdbc173bfb17b859
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438848"
---
# <a name="ftp-append"></a>FTP: append

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

acrescenta um arquivo local em um arquivo no computador remoto usando a configuração atual do tipo arquivo.   
## <a name="syntax"></a>Sintaxe  
```  
append <LocalFile> [remoteFile]  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                               Descrição                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Especifica o arquivo local para adicionar.                     |
| [remoteFile] | Especifica o arquivo no computador remoto para o qual <LocalFile> é adicionado. |

## <a name="remarks"></a>Comentários  
Se *Arquivo_remoto* for omitido, o *LocalFile* nome é usado no lugar do nome de arquivo remoto.  
## <a name="BKMK_Examples"></a>Exemplos  
Acrescente file1.txt para file2.txt no computador remoto.  
```  
append file1.txt file2.txt  
```  
acrescente a file1.txt local em um arquivo chamado file1.txt no computador remoto.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
