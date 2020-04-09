---
title: acréscimo de FTP
description: Tópico de comandos do Windows para o FTP Append
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44ce1d6e7259dc8745da35ed462e6378f0fce8ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843819"
---
# <a name="ftp-append"></a>FTP: acrescentar

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
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
