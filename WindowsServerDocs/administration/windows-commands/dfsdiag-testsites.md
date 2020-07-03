---
title: dfsdiag testsites
description: Artigo de referência para Dfsdiag testsites, que verifica a configuração dos sites dos serviços de domínio Active Directory (AD DS) verificando se os servidores que atuam como servidores de namespace ou destinos de pasta (link) têm as mesmas associações de site em todos os controladores de domínio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7942b1535957366af9485580d75c9eec17120f4d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928681"
---
# <a name="dfsdiag-testsites"></a>dfsdiag testsites

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração dos sites dos serviços de domínio do Active Directory (AD DS) verificando se os servidores que atuam como servidores de namespace ou destinos de pasta (link) têm as mesmas associações de site em todos os controladores de domínio.

## <a name="syntax"></a>Sintaxe

```
dfsdiag /testsites </machine:<server name>| /DFSpath:<namespace root or DFS folder> [/recurse]> [/full]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `/machine:<server name>` | O nome do servidor no qual verificar a associação do site. |
| `/DFSpath:<namespace root or DFS folder>` | A raiz do namespace ou a pasta do Sistema de Arquivos Distribuído (DFS) (link) com destinos para os quais verificar a associação do site. |
| /recurse | Enumera e verifica as associações de site para todos os destinos de pasta na raiz de namespace especificada. |
| /full | Verifica se AD DS e o registro do servidor contêm as mesmas informações de associação do site. |

## <a name="examples"></a>Exemplos

Para verificar as associações de site em *machine\MyServer*, digite:

```
dfsdiag /testsites /machine:MyServer
```

Para verificar uma pasta Sistema de Arquivos Distribuído (DFS) para verificar a associação do site, juntamente com a verificação de que AD DS e o registro do servidor contêm as mesmas informações de associação do site, digite:

```
dfsdiag /TestSites /DFSpath:\\contoso.com\namespace1\folder1 /full
```

Para verificar uma raiz de namespace para verificar a associação do site, juntamente com a enumeração e a verificação das associações de site para todos os destinos de pasta na raiz de namespace especificada e a verificação de que AD DS e o registro do servidor contêm as mesmas informações de associação do site, digite:

```
dfsdiag /testsites /DFSpath:\\contoso.com\namespace2 /recurse /full
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Dfsdiag](dfsdiag.md)
