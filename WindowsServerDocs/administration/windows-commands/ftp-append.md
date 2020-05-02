---
title: acréscimo de FTP
description: Tópico de referência para Append do FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b53228473b8ea16a0955c244d60fae77cf4f7d7f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725401"
---
# <a name="ftp-append"></a>FTP: acrescentar

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

anexa um arquivo local a um arquivo no computador remoto usando a configuração de tipo de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
append <LocalFile> [remoteFile]  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                               Descrição                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Especifica o arquivo local a ser adicionado.                     |
| [remotofile] | Especifica o arquivo no computador remoto ao qual <LocalFile> é adicionado. |

## <a name="remarks"></a>Comentários  
Se *remotefile* for omitido, o nome de *LocalFile* será usado no lugar do nome do arquivo remoto.  
## <a name="examples"></a>Exemplos  
Acrescente file1. txt a file2. txt no computador remoto.  
```  
append file1.txt file2.txt  
```  
Acrescente o arquivo1. txt local a um arquivo chamado arquivo1. txt no computador remoto.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
