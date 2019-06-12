---
title: ftp mdir
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c4ec445c3e367a46dc40d10a37c0b3b8e53a10e3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438335"
---
# <a name="ftp-mdir"></a>ftp: mdir

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de diretórios de arquivos e subdiretórios em um diretório remoto.   
## <a name="syntax"></a>Sintaxe  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                               Descrição                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Especifica o diretório ou arquivo para o qual você deseja ver uma listagem.   |
| <LocalFile>  | Especifica um arquivo local para armazenar a listagem. Este parâmetro é necessário. |

## <a name="remarks"></a>Comentários  
- Você pode usar **mdir** para especificar vários arquivos.  
- Especificando *Arquivo_remoto*  
  Digite um hífen ( **-** ) para usar o diretório de trabalho atual no computador remoto.  
- Especificando um *LocalFile*  
  Digite um hífen ( **-** ) para exibir a listagem na tela.  
  ## <a name="BKMK_Examples"></a>Exemplos  
  Exibir uma lista de diretório **dir1** e **dir2** na tela  
  ```  
  mdir dir1 dir2 -  
  ```  
  Salve a listagem de diretório combinado dos **dir1** e **dir2** em um arquivo local chamado **Lista_pastas**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
