---
title: query termserver
description: Artigo de referência para o comando Query termserver, que exibe uma lista de todos os servidores de Host da Sessão da Área de Trabalho Remota na rede.
ms.topic: reference
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 103e1c888f20e368e014eba762b739e099d9e491
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038394"
---
# <a name="query-termserver"></a>query termserver

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de todos os servidores de Host da Sessão da Área de Trabalho Remota na rede. Esse comando pesquisa a rede em busca de todos os servidores Host da Sessão da Área de Trabalho Remota conectados e retorna as seguintes informações:

- Nome do servidor

- Rede (e endereço de nó se a opção/address for usada)

> [!NOTE]
> Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
query termserver [<servername>] [/domain:<domain>] [/address] [/continue]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<servername>` | Especifica o nome que identifica o servidor de Host da Sessão da Área de Trabalho Remota. |
| /Domain`<domain>` | Especifica o domínio para consultar os servidores de terminal. Você não precisará especificar um domínio se estiver consultando o domínio no qual você está trabalhando no momento. |
| /address | Exibe os endereços de rede e de nó para cada servidor. |
| /continue | Impede a pausa após a exibição de cada tela de informações. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para exibir informações sobre todos os servidores de Host da Sessão da Área de Trabalho Remota na rede, digite:

```
query termserver
```

Para exibir informações sobre o servidor de Host da Sessão da Área de Trabalho Remota chamado *Server3*, digite:

```
query termserver Server3
```

Para exibir informações sobre todos os servidores de Host da Sessão da Área de Trabalho Remota no domínio *contoso*, digite:

```
query termserver /domain:CONTOSO
```

Para exibir a rede e o endereço do nó para o servidor de Host da Sessão da Área de Trabalho Remota chamado *Server3*, digite:

```
query termserver Server3 /address
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de consulta](query.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
