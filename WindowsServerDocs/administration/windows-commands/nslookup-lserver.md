---
title: nslookup lserver
description: Artigo de referência para o comando do lserver do nslookup, que altera o servidor inicial para o domínio DNS (sistema de nomes de domínio) especificado.
ms.topic: reference
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d1f507b1a61e9fd4fc506fb4e74f869c83e95335
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635669"
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
