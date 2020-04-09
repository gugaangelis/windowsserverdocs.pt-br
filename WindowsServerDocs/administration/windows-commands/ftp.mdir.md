---
title: mdir FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb024d063761f1e817f02fdd7301376dd167846
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842709"
---
# <a name="ftp-mdir"></a>FTP: mdir

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de diretórios de arquivos e subdiretórios em um diretório remoto.   
## <a name="syntax"></a>Sintaxe  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                               Descrição                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Especifica o diretório ou o arquivo para o qual você deseja ver uma listagem.   |
| <LocalFile>  | Especifica um arquivo local para armazenar a listagem. Esse parâmetro é necessário. |

## <a name="remarks"></a>Comentários  
- Você pode usar **mdir** para especificar vários arquivos.  
- Especificando *remotefile*  
  Digite um hífen ( **-** ) para usar o diretório de trabalho atual no computador remoto.  
- Especificando um *LocalFile*  
  Digite um hífen ( **-** ) para exibir a listagem na tela.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
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
