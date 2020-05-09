---
title: dfsdiag testdfsconfig
description: Tópico de referência para o Dfsdiag testdfsconfig, que verifica a configuração de um namespace de Sistema de Arquivos Distribuído (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d9490f35c2d509c83d9008aa87627bd3c55a875
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992990"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag testdfsconfig

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração de um namespace de Sistema de Arquivos Distribuído (DFS) executando as seguintes ações:

- Verifica se o serviço de namespace do DFS está em execução e se seu tipo de inicialização está definido como **automático** em todos os servidores de namespace.

- Verifica se a configuração do registro DFS é consistente entre os servidores de namespace.

- Valida as seguintes dependências em servidores de namespace clusterizados:

  - Dependência de recurso raiz de namespace no recurso de nome de rede.

  - Dependência de recurso de nome de rede no recurso de endereço IP.

  - Dependência de recurso raiz de namespace no recurso de disco físico.

## <a name="syntax"></a>Sintaxe

```
dfsdiag /testdfsconfig /DFSroot:<namespace>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /DFSroot:`<namespace>` | O namespace (raiz DFS) a ser diagnosticado. |

## <a name="examples"></a>Exemplos

Para verificar a configuração de namespaces de Sistema de Arquivos Distribuído (DFS) em *contoso. com\MyNamespace*, digite:

```
dfsdiag /testdfsconfig /DFSroot:\contoso.com\MyNamespace
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Dfsdiag](dfsdiag.md)
