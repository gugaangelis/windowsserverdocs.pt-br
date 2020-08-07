---
title: dfsdiag
description: Artigo de referência para o comando Dfsdiag, que fornece informações de diagnóstico para namespaces do DFS.
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea349e088ccecd772130d30bfba01cbd1bf2e8e6
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891055"
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
