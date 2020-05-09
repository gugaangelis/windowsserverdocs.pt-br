---
title: DFSUtil
description: Tópico de referência para o comando Dfsutil, que gerencia namespaces do DFS, servidores e clientes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6905d90ee42958e47dfed4869520000a4fd3ddf
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992620"
---
# <a name="dfsutil"></a>DFSUtil

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando Dfsutil gerencia namespaces, servidores e clientes do DFS.

## <a name="functionality-available-in-powershell"></a>Funcionalidade disponível no PowerShell

O módulo [DFSN](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps) do PowerShell fornece funcionalidade equivalente aos seguintes parâmetros DFSUtil.

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

Para obter mais informações sobre cada um desses comandos, abra um prompt de comando em um servidor com as ferramentas de gerenciamento de namespaces do DFS `dfsutil client /?`instaladas e, em seguida, digite, `dfsutil diag /?`ou `dfsutil cache /?`.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)