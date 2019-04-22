---
title: dfsdiag
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab5c86ce7ed4760aef4941de55e8dcf8efe48c8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819137"
---
# <a name="dfsdiag"></a>dfsdiag



O `Dfsdiag` comando fornece informações de diagnóstico para Namespaces do DFS.

## <a name="syntax"></a>Sintaxe

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|Verifica a configuração do controlador de domínio.|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|Verificações de associações do site.|
|[dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|Verifica a configuração de Namespace DFS.|
|[dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|Verifica a integridade do Namespace do DFS.|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|Verifica as respostas de referência.|
|/?|Exibe a ajuda no prompt de comando.|

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)