---
title: DFSUtil
description: Artigo de referência para o comando Dfsutil, que gerencia namespaces do DFS, servidores e clientes.
ms.topic: reference
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9fb3883403dd0f4a88b612655247c4f4a2df7de7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635047"
---
# <a name="dfsutil"></a>DFSUtil

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando Dfsutil gerencia namespaces, servidores e clientes do DFS.

## <a name="functionality-available-in-powershell"></a>Funcionalidade disponível no PowerShell

O módulo [DFSN](/powershell/module/dfsn/?view=win10-ps) do PowerShell fornece funcionalidade equivalente aos seguintes parâmetros DFSUtil.

| Parâmetro | Descrição |
| --------- | ----------- |
| root | Exibe, cria, remove, importa e exporta raízes de namespace. |
| link | Exibe, cria, remove ou move pastas (links). |
| destino | Exibe, cria, remove o destino da pasta ou o servidor de namespace. |
| propriedade | Exibe ou modifica um destino de pasta ou servidor de namespace. |
| Servidor | Exibe ou modifica a configuração do namespace. |
| domínio | Exibe todos os namespaces baseados em domínio em um domínio. |

## <a name="functionality-available-only-in-dfsutil"></a>Funcionalidade disponível somente em Dfsutil

A funcionalidade a seguir está disponível apenas como parâmetros Dfsutil:

| Parâmetro | Descrição |
| --------- | ----------- |
| cliente | Exibe ou modifica informações do cliente ou chaves do registro. |
| diag | Executar diagnóstico ou exibir dfsdirs/dfspath. |
| cache | Exibe ou libera o cache do cliente. |

Para obter mais informações sobre cada um desses comandos, abra um prompt de comando em um servidor com as ferramentas de gerenciamento de namespaces do DFS instaladas e, em seguida, digite `dfsutil client /?` , `dfsutil diag /?` ou `dfsutil cache /?` .

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
