---
title: nslookup lserver
description: Artigo de referência para o comando do lserver do nslookup, que altera o servidor inicial para o domínio DNS (sistema de nomes de domínio) especificado.
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7c478b6c9f3ae299559a556629e53f9eb852ee
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885797"
---
# <a name="nslookup-lserver"></a>nslookup lserver

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor inicial para o domínio DNS (sistema de nomes de domínio) especificado.

Esse comando usa o servidor inicial para pesquisar as informações sobre o domínio DSN especificado. Se você quiser pesquisar informações usando o servidor padrão atual, use o comando do [servidor nslookup](nslookup-server.md) .

## <a name="syntax"></a>Sintaxe

```
lserver <DNSdomain>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<DNSdomain>` | Especifica o domínio DNS para o servidor inicial. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup server](nslookup-server.md)
