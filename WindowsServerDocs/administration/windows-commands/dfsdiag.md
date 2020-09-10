---
title: dfsdiag
description: Artigo de referência para o comando Dfsdiag, que fornece informações de diagnóstico para namespaces do DFS.
ms.topic: reference
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 05ca89a2ad2984ec7e47002489f26968e7d2d69b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635029"
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
