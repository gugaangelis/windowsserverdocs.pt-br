---
title: acréscimo de FTP
description: 'Tópico de comandos do Windows para o FTP Append '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 52d16b878ff5fb165fd851b227dcc361c9da3a80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376637"
---
# <a name="ftp-append"></a>FTP: acrescentar

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

anexa um arquivo local a um arquivo no computador remoto usando a configuração de tipo de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
append <LocalFile> [remoteFile]  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                               Descrição                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Especifica o arquivo local a ser adicionado.                     |
| [remotofile] | Especifica o arquivo no computador remoto ao qual <LocalFile> é adicionado. |

## <a name="remarks"></a>Comentários  
Se *remotefile* for omitido, o nome de *LocalFile* será usado no lugar do nome do arquivo remoto.  
## <a name="BKMK_Examples"></a>Disso  
Acrescente file1. txt a file2. txt no computador remoto.  
```  
append file1.txt file2.txt  
```  
Acrescente o arquivo1. txt local a um arquivo chamado arquivo1. txt no computador remoto.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
