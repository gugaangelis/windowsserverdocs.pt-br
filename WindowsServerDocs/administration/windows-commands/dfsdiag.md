---
title: dfsdiag
description: Artigo de referência para o comando Dfsdiag, que fornece informações de diagnóstico para namespaces do DFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97f39d740bc321ebcece69ff0690dfac7aab6567
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928667"
---
# <a name="dfsdiag"></a>dfsdiag

Fornece informações de diagnóstico para namespaces do DFS.

## <a name="syntax"></a>Sintaxe

```
dfsdiag /testdcs [/domain:<domain name>]
dfsdiag /testsites </machine:<server name>| /DFSPath:<namespace root or DFS folder> [/recurse]> [/full]
dfsdiag /testdfsconfig /DFSRoot:<namespace>
dfsdiag /testdfsintegrity /DFSRoot:<DFS root path> [/recurse] [/full]
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| [dfsdiag testdcs](dfsdiag-testdcs.md) | Verifica a configuração do controlador de domínio. |
| [dfsdiag testsites](dfsdiag-testsites.md) | Verifica as associações do site. |
| [dfsdiag testdfsconfig](dfsdiag-testdfsconfig.md) | Verifica a configuração do namespace do DFS. |
| [dfsdiag testdfsintegrity](dfsdiag-testdfsintegrity.md) | Verifica a integridade do namespace do DFS. |
| [dfsdiag testreferral](dfsdiag-testreferral.md) | Verifica as respostas de referência. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
