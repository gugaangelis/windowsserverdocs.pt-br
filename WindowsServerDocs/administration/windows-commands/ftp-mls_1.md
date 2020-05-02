---
title: mls_1 FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27f74d4c1d03cb4d9f665566f69485e80f8eccdc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725201"
---
# <a name="ftp-mls_1"></a>FTP: mls_1

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista abreviada de arquivos e subdiretórios em um diretório remoto.   
## <a name="syntax"></a>Sintaxe  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                       Descrição                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | Especifica o arquivo para o qual você deseja ver uma listagem. |
| <LocalFile>  |  Especifica um arquivo local no qual armazenar a listagem.  |

## <a name="remarks"></a>Comentários  
- Especificando *remoteFiles*  
  Digite um hífen (**-**) para usar o diretório de trabalho atual no computador remoto.  
- Especificando o *LocalFile*  
  Digite um hífen (**-**) para exibir a listagem na tela.  
  ## <a name="examples"></a>Exemplos  
  Exiba uma lista abreviada de arquivos e subdiretórios para **dir1** e **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Salve uma lista abreviada de arquivos e subdiretórios para **dir1** e **dir2** no arquivo local **DirList. txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
