---
title: dfsdiag
description: Tópico de referência para Dfsdiag, que fornece informações de diagnóstico para namespaces do DFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d5a9b147994628ccad6a723311decbccbe82ec6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719548"
---
# <a name="dfsdiag"></a>dfsdiag

Fornece informações de diagnóstico para namespaces do DFS.

## <a name="syntax"></a>Sintaxe

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|Verifica a configuração do controlador de domínio.|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|Verifica as associações do site.|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|Verifica a configuração do namespace do DFS.|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|Verifica a integridade do namespace do DFS.|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|Verifica as respostas de referência.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)