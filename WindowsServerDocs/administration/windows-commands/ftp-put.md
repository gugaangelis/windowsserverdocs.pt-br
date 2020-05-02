---
title: Put FTP
description: Tópico de comandos do Windows para FTP-put
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: d86ba8418f4c43c26f3745a9f70e676ca790c640
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725173"
---
# <a name="ftp-put"></a>FTP: Put

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.
## <a name="syntax"></a>Sintaxe
```
put <LocalFile> [<remoteFile>]
```
#### <a name="parameters"></a>Parâmetros

|    Parâmetro     |                    Descrição                    |
|------------------|---------------------------------------------------|
|   `<LocalFile>`  |         Especifica o arquivo local a ser copiado.         |
| `[<remoteFile>]` | Especifica o nome a ser usado no computador remoto. |

## <a name="remarks"></a>Comentários
- O comando **Put** é idêntico ao comando **Send** .
- Se *remotefile* não for especificado, o arquivo receberá o nome de *LocalFile* .
  ## <a name="examples"></a>Exemplos
  Copie o arquivo local **Test. txt** e nomeie-o **Test1. txt** no computador remoto.
  ```
  put test.txt test1.txt
  ```
  Copie o arquivo local **Program. exe** para o computador remoto.
  ```
  put program.exe
  ```
  ## <a name="additional-references"></a>Referências adicionais
- [FTP: ASCII](ftp-ascii.md)
- [FTP: binário](ftp-binary.md)
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
