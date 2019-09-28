---
title: mls_1 FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a84a0f8f3121ea19876744e9ef04bebf5f9fcb08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376262"
---
# <a name="ftp-mls_1"></a>FTP: mls_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista abreviada de arquivos e subdiretórios em um diretório remoto.   
## <a name="syntax"></a>Sintaxe  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                       Descrição                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | Especifica o arquivo para o qual você deseja ver uma listagem. |
| <LocalFile>  |  Especifica um arquivo local no qual armazenar a listagem.  |

## <a name="remarks"></a>Comentários  
- Especificando *remoteFiles*  
  Digite um hífen ( **-** ) para usar o diretório de trabalho atual no computador remoto.  
- Especificando o *LocalFile*  
  Digite um hífen ( **-** ) para exibir a listagem na tela.  
  ## <a name="BKMK_Examples"></a>Disso  
  Exiba uma lista abreviada de arquivos e subdiretórios para **dir1** e **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Salve uma lista abreviada de arquivos e subdiretórios para **dir1** e **dir2** no arquivo local **DirList. txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
