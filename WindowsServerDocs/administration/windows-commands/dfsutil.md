---
title: DFSUtil
description: Tópico de referência para Dfsutil, que gerencia namespaces do DFS, servidores e clientes. os comandos Dfsutil usam a terminologia original do Sistema de Arquivos Distribuído, com a terminologia atualizada de namespaces do DFS fornecida como explicação para a maioria dos comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 999eef79227d4531ba724c9cac40127297ea38a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719520"
---
# <a name="dfsutil"></a>DFSUtil

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando Dfsutil gerencia namespaces, servidores e clientes do DFS.

>[!NOTE]
>O **módulo namespaces DFS do PowerShell** fornece substituições para alguns dos parâmetros Dfsutil, enquanto outros ainda exigem que você use DFSUtil. Para obter mais informações sobre os equivalentes do PowerShell atualizados, consulte [DFSN](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps).

## <a name="parameters-available-in-powershell"></a>Parâmetros disponíveis no PowerShell

Você pode usar os seguintes parâmetros do PowerShell:

| Parâmetro | Descrição |
| --------- | ----------- |
| root | Exibe, cria, remove, importa e exporta raízes de namespace. |
| link | Exibe, cria, remove ou move pastas (links). |
| destino | Exibe, cria, remove o destino da pasta ou o servidor de namespace. |
| propriedade | Exibe ou modifica um destino de pasta ou servidor de namespace. |
| Servidor | Exibe ou modifica a configuração do namespace. |
| domínio | Exibe todos os namespaces baseados em domínio em um domínio. |

## <a name="parameters-only-available-in-dfsutil"></a>Parâmetros disponíveis somente em Dfsutil

Você pode usar os parâmetros a seguir somente de DFSUtil.

| Parâmetro | Descrição |
| --------- | ----------- |
| cliente | Exibe ou modifica informações do cliente ou chaves do registro. |
| diag | Executar diagnóstico ou exibir dfsdirs/dfspath. |
| cache | Exibe ou libera o cache do cliente. |

Para obter mais informações sobre cada um desses comandos, abra um prompt de comando em um servidor com as ferramentas de gerenciamento de namespaces do DFS `dfsutil client /?`instaladas e, em seguida, digite, `dfsutil diag /?`ou `dfsutil cache /?`.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)