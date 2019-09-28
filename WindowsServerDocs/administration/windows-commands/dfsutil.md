---
title: DFSUtil
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
robots: noindex,nofollow
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a06806b109bbd324213f935892bbbab415362df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377983"
---
# <a name="dfsutil"></a>DFSUtil

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando Dfsutil gerencia namespaces, servidores e clientes do DFS. os comandos Dfsutil usam a terminologia original do Sistema de Arquivos Distribuído, com a terminologia atualizada de namespaces do DFS fornecida como explicação para a maioria dos comandos.

para obter exemplos de como esse comando pode ser usado, consulte 

## <a name="syntax"></a>Sintaxe

```
command </parameter> </param2>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|[DFSUtil root](dfsutil-root.md)|Exibe, cria, remove, importa e exporta raízes de namespace.|
|[DFSUtil link](dfsutil-link.md)|Exibe, cria, remove ou move pastas \(links @ no__t-1.|
|[Destino Dfsutil](dfsutil-target.md)|Exibe, cria, remove o destino da pasta ou o servidor de namespace.|
|[Propriedade Dfsutil](dfsutil-property.md)|Exibe ou modifica um destino de pasta ou servidor de namespace.|
|[Cliente Dfsutil](dfsutil-client.md)|Exibe ou modifica informações do cliente ou chaves do registro.|
|[DFSUtil Server](dfsutil-server.md)|Exibe ou modifica a configuração do namespace.|
|[DFSUtil diag](dfsutil-diag.md)|Execute Diagnostics ou exiba dfsdirs @ no__t-0dfspath.|
|[DFSUtil Domain](dfsutil-domain.md)|Exibe todos os namespaces @ no__t-0based de domínio em um domínio.|
|[Cache Dfsutil](dfsutil-cache.md)|Exibe ou libera o cache do cliente.|
|[DFSUtil oldcli](dfsutil-oldcli.md)|Use o comando Dfsutil \/oldcli para usar a sintaxe Dfsutil original.|

## <a name="remarks-optional-section"></a>Comentários <optional section>
Se você especificar um objeto \(such como um servidor de namespace @ no__t-1 no final de um comando, a maioria dos comandos exibirá informações sobre o objeto sem exigir mais parâmetros ou comandos. Por exemplo, ao usar o comando Dfsutil root, você pode acrescentar uma raiz de namespace ao comando para exibir informações sobre a raiz.

## <a name="BKMK_Examples"></a>Disso
&lt;Here é onde você coloca uma descrição detalhada do seu exemplo. &gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;Here é onde você coloca uma descrição detalhada de outro exemplo. &gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)


