---
title: dfsutil
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 45545b4e12d31c293ead5b18b83efd50d7bc37bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439654"
---
# <a name="dfsutil"></a>dfsutil

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando dfsutil gerencia os Namespaces do DFS, servidores e clientes. comandos Dfsutil usam a terminologia original do sistema de arquivos distribuído, com a terminologia de Namespaces do DFS atualizada fornecida como explicação para a maioria dos comandos.

Para obter exemplos de como esse comando pode ser usado, consulte 

## <a name="syntax"></a>Sintaxe

```
command </parameter> </param2>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|[dfsutil Root](dfsutil-root.md)|Exibe, cria, remove, importa, exporta as raízes de namespace.|
|[dfsutil Link](dfsutil-link.md)|Exibe, cria, remove ou move pastas \(links\).|
|[dfsutil Target](dfsutil-target.md)|Monitores, criar, remover o servidor de destino ou o namespace da pasta.|
|[Dfsutil propriedade](dfsutil-property.md)|Exibe ou modifica um servidor de namespace ou destino de pasta.|
|[dfsutil Client](dfsutil-client.md)|Exibe ou modifica as chaves de registro ou de informações do cliente.|
|[dfsutil Server](dfsutil-server.md)|Exibe ou modifica a configuração de namespace.|
|[Dfsutil Diag](dfsutil-diag.md)|Execute o diagnóstico ou exibir dfsdirs\/dfspath.|
|[dfsutil Domain](dfsutil-domain.md)|Exibe todos os domínio\-com base em namespaces em um domínio.|
|[dfsutil Cache](dfsutil-cache.md)|Exibe ou libera o cache do cliente.|
|[dfsutil oldcli](dfsutil-oldcli.md)|Usar o dfsutil \/oldcli comando para usar a sintaxe dfsutil original.|

## <a name="remarks-optional-section"></a>Comentários <optional section>
Se você especificar um objeto \(como um servidor de namespace\) no final de um comando, a maioria dos comandos serão exibidas informações sobre o objeto sem a necessidade de mais comandos ou parâmetros. Por exemplo, ao usar o comando raiz dfsutil, você pode acrescentar uma raiz do namespace para o comando para exibir informações sobre a raiz.

## <a name="BKMK_Examples"></a>Exemplos
&lt;Aqui é onde você coloca uma descrição detalhada do seu exemplo.&gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;Aqui é onde você coloca uma descrição detalhada do outro exemplo.&gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)


