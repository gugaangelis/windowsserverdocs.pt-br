---
title: nslookup lserver
description: Artigo de referência para o comando do lserver do nslookup, que altera o servidor inicial para o domínio DNS (sistema de nomes de domínio) especificado.
ms.topic: reference
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99d3dbeac4073b35abd540c185e4cf69b723b61e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023470"
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
