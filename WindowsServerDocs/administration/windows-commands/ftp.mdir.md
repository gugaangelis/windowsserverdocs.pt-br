---
title: mdir FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08aa5bb216a3d0155c100c761e476bb963e59311
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375853"
---
# <a name="ftp-mdir"></a>FTP: mdir

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de diretórios de arquivos e subdiretórios em um diretório remoto.   
## <a name="syntax"></a>Sintaxe  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                               Descrição                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Especifica o diretório ou o arquivo para o qual você deseja ver uma listagem.   |
| <LocalFile>  | Especifica um arquivo local para armazenar a listagem. Este parâmetro é necessário. |

## <a name="remarks"></a>Comentários  
- Você pode usar **mdir** para especificar vários arquivos.  
- Especificando *remotefile*  
  Digite um hífen ( **-** ) para usar o diretório de trabalho atual no computador remoto.  
- Especificando um *LocalFile*  
  Digite um hífen ( **-** ) para exibir a listagem na tela.  
  ## <a name="BKMK_Examples"></a>Disso  
  Exibir uma listagem de diretório de **dir1** e **dir2** na tela  
  ```  
  mdir dir1 dir2 -  
  ```  
  Salve a listagem de diretório combinada de **dir1** e **dir2** em um arquivo local chamado **DirList. txt**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
