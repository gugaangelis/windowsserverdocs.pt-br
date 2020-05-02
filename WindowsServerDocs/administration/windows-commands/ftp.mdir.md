---
title: mdir FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54adcd1d28079487c5e238c72413d8269447944e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725022"
---
# <a name="ftp-mdir"></a>FTP: mdir

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de diretórios de arquivos e subdiretórios em um diretório remoto.   
## <a name="syntax"></a>Sintaxe  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                               Descrição                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Especifica o diretório ou o arquivo para o qual você deseja ver uma listagem.   |
| <LocalFile>  | Especifica um arquivo local para armazenar a listagem. Este parâmetro é necessário. |

## <a name="remarks"></a>Comentários  
- Você pode usar **mdir** para especificar vários arquivos.  
- Especificando *remotefile*  
  Digite um hífen (**-**) para usar o diretório de trabalho atual no computador remoto.  
- Especificando um *LocalFile*  
  Digite um hífen (**-**) para exibir a listagem na tela.  
  ## <a name="examples"></a>Exemplos  
  Exibir uma listagem de diretório de **dir1** e **dir2** na tela  
  ```  
  mdir dir1 dir2 -  
  ```  
  Salve a listagem de diretório combinada de **dir1** e **dir2** em um arquivo local chamado **DirList. txt**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
