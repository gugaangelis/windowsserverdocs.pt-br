---
title: mls_1 FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca3b8e04dd4a152b2d1bf8ce1ca8006d70186116
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843289"
---
# <a name="ftp-mls_1"></a>FTP: mls_1

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
  Digite um hífen ( **-** ) para usar o diretório de trabalho atual no computador remoto.  
- Especificando o *LocalFile*  
  Digite um hífen ( **-** ) para exibir a listagem na tela.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
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
