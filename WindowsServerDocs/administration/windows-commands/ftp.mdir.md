---
title: ftp mdir
description: Artigo de referência para o comando FTP mdir, que exibe uma lista de diretórios de arquivos e subdiretórios em um diretório remoto.
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddd4a5adb282464d2b5a202ea8ed2838be7a4676
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888720"
---
# <a name="ftp-mdir"></a>ftp mdir

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de diretórios de arquivos e subdiretórios em um diretório remoto.

## <a name="syntax"></a>Sintaxe

```
mdir <remotefile>[...] <localfile>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<remotefile>` | Especifica o diretório ou o arquivo para o qual você deseja ver uma listagem. Você pode especificar vários *remotefiles*. Digite um hífen (-) para usar o diretório de trabalho atual no computador remoto. |
| `<localfile>` | Especifica um arquivo local para armazenar a listagem. Este parâmetro é necessário. Digite um hífen (-) para exibir a listagem na tela. |

### <a name="examples"></a>Exemplos

Para exibir uma listagem de diretório de *dir1* e *dir2* na tela, digite:

```
mdir dir1 dir2 -
```

Para salvar a listagem de diretório combinada de *dir1* e *dir2* em um arquivo local chamado *dirlist.txt*, digite:

```
mdir dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
