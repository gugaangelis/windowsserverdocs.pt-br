---
title: bitsadmin setcustomheaders
description: Tópico de comandos do Windows para **Bitsadmin setcustomheaders**, que adiciona um cabeçalho HTTP personalizado a uma solicitação get.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5b1a28f03815a22a3f8d10b2c3d1d4a3a2ae635
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123021"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

Adicione um cabeçalho HTTP personalizado a uma solicitação GET enviada a um servidor HTTP.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| `<header1> <header2>` e assim por diante | Os cabeçalhos personalizados para o trabalho. |

## <a name="examples"></a>Exemplos

O exemplo a seguir adiciona um cabeçalho HTTP personalizado para o trabalho chamado *myDownloadJob*. Para obter mais informações sobre solicitações GET, consulte [definições de método](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3) e definições de campo de [cabeçalho](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

```
C:\>bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)