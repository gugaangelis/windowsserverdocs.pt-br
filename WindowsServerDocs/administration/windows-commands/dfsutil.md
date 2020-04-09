---
title: DFSUtil
description: Tópico de comandos do Windows para Dfsutil, que gerencia namespaces do DFS, servidores e clientes. os comandos Dfsutil usam a terminologia original do Sistema de Arquivos Distribuído, com a terminologia atualizada de namespaces do DFS fornecida como explicação para a maioria dos comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30415bc85fd8a4a4804946a3d4a168d6a7d1433a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845579"
---
# <a name="dfsutil"></a>DFSUtil

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando Dfsutil gerencia namespaces, servidores e clientes do DFS. Na maioria das vezes, você pode usar os cmdlets do PowerShell de namespaces do DFS mais recentes, embora haja alguns comandos que ainda exigem DFSUtil.

## <a name="parameters-available-in-powershell"></a>Parâmetros disponíveis no PowerShell

Você pode usar os seguintes parâmetros do PowerShell:

| Parâmetro | Descrição |
| --------- | ----------- |
| root | Exibe, cria, remove, importa e exporta raízes de namespace. |
| link | Exibe, cria, remove ou move pastas (links). |
| target | Exibe, cria, remove o destino da pasta ou o servidor de namespace. |
| {1&gt;propriedade&lt;1} | Exibe ou modifica um destino de pasta ou servidor de namespace. |
| server | Exibe ou modifica a configuração do namespace. |
| domain | Exibe todos os namespaces baseados em domínio em um domínio. |

## <a name="parameters-only-available-in-dfsutil"></a>Parâmetros disponíveis somente em Dfsutil

Você pode usar os parâmetros a seguir somente de DFSUtil.

| Parâmetro | Descrição |
| --------- | ----------- |
| Cliente | Exibe ou modifica informações do cliente ou chaves do registro. |
| diag | Executar diagnóstico ou exibir dfsdirs/dfspath. |
| cache | Exibe ou libera o cache do cliente. |

Para obter mais informações sobre cada um desses comandos, abra um prompt de comando em um servidor com as ferramentas de gerenciamento de namespaces do DFS instaladas e digite `dfsutil client /?`, `dfsutil diag /?`ou `dfsutil cache /?`.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)