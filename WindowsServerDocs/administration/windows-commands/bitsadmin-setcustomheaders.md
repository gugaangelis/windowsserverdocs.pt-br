---
title: bitsadmin setcustomheaders
description: Tópico de referência para o comando Bitsadmin setcustomheaders, que adiciona um cabeçalho HTTP personalizado a uma solicitação GET.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92728f8d63a22cf9d13d6c02a69359583a9fc5cc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719331"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

Adicione um cabeçalho HTTP personalizado a uma solicitação GET enviada a um servidor HTTP. Para obter mais informações sobre solicitações GET, consulte [definições de método](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3) e definições de campo de [cabeçalho](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| `<header1> <header2>`e assim por diante | Os cabeçalhos personalizados para o trabalho. |

## <a name="examples"></a>Exemplos

Para adicionar um cabeçalho HTTP personalizado para o trabalho chamado *myDownloadJob*:

```
bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
