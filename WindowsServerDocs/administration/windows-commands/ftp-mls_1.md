---
title: ftp mls
description: Artigo de referência para o comando FTP MLS, que exibe uma lista abreviada de arquivos e subdiretórios em um diretório remoto.
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c2e0dc4ff4b516cc436e5b8a0a9c5ca2e4d77b5d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889241"
---
# <a name="ftp-mls"></a>ftp mls

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista abreviada de arquivos e subdiretórios em um diretório remoto.

## <a name="syntax"></a>Sintaxe

```
mls <remotefile>[ ] <localfile>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<remotefile>` | Especifica o arquivo para o qual você deseja ver uma listagem. Ao especificar *remotefiles*, use um hífen para representar o diretório de trabalho atual no computador remoto. |
| `<localfile>` | Especifica um arquivo local no qual armazenar a listagem. Ao especificar o *LocalFile*, use um hífen para exibir a listagem na tela. |

### <a name="examples"></a>Exemplos

Para exibir uma lista abreviada de arquivos e subdiretórios para *dir1* e *dir2*, digite:

```
mls dir1 dir2 -
```

Para salvar uma lista abreviada de arquivos e subdiretórios para *dir1* e *dir2* no arquivo local *dirlist.txt*, digite:

```
mls dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
