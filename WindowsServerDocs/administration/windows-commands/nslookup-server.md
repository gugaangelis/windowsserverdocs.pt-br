---
title: nslookup server
description: Artigo de referência para o comando do servidor Nslookup, que altera o servidor padrão para o domínio DNS (sistema de nomes de domínio) especificado.
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eacb1807810627956fcf75455e861d3ac381cf13
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885783"
---
# <a name="nslookup-server"></a>nslookup server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o domínio DNS (sistema de nomes de domínio) especificado.

Esse comando usa o servidor padrão atual para pesquisar as informações sobre o domínio DSN especificado. Se você quiser pesquisar informações usando o servidor inicial, use o comando [nslookup lserver](nslookup-lserver.md) .

## <a name="syntax"></a>Sintaxe

```
server <DNSdomain>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<DNSdomain>` | Especifica o domínio DNS para o servidor padrão. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup lserver](nslookup-lserver.md)
