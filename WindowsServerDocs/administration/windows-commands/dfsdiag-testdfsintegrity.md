---
title: dfsdiag TestDFSIntegrity
description: Tópico de comandos do Windows para **Dfsdiag TestDFSIntegrity**, que verifica a integridade do namespace do sistema de arquivos distribuído (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 714b79369898338a4e4a6e4fad8487709ab4fc60
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846269"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag TestDFSIntegrity

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a integridade do namespace do Sistema de Arquivos Distribuído (DFS) executando os seguintes testes:

- Verifica se há danos ou inconsistências nos metadados do DFS entre os controladores de domínio.

- Valida a configuração da enumeração baseada em acesso para garantir que ela seja consistente entre os metadados do DFS e o compartilhamento do servidor de namespace.

- Detecta pastas DFS sobrepostas (links), pastas duplicadas e pastas com destinos de pasta sobrepostos.

## <a name="syntax"></a>Sintaxe

```
dfsdiag /TestDFSIntegrity /DFSRoot: <DFS root path> [/Recurse] [/Full]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|-------|--------|
| /DFSRoot: `<DFS root path>`| O namespace do DFS para diagnosticar. |
| /Recurse | Executa os testes, incluindo os Interlinks de namespace. |
| /Full | Verifica a consistência do compartilhamento e das ACLs de NTFS e da configuração do lado do cliente em todos os destinos de pasta. Ele também verifica se a propriedade online está definida. |

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

```
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

-   [dfsdiag](dfsdiag.md)


