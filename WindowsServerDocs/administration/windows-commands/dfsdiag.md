---
title: dfsdiag
description: Artigo de referência para o comando Dfsdiag, que fornece informações de diagnóstico para namespaces do DFS.
ms.topic: reference
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da34cc9b4c2cfcb30d2f8ff3161d6777ae9d0275
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034144"
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
