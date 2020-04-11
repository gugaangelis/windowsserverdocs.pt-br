---
title: bitsadmin sethttpmethod
description: Tópico de comandos do Windows para **Bitsadmin sethttpmethod**, que define o verbo http a ser usado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: d349dcad7bdf6a6fc566ed961c3160836d7f49da
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122962"
---
# <a name="bitsadmin-sethttpmethod"></a>bitsadmin sethttpmethod

Define o verbo HTTP a ser usado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| HttpMethod | O verbo HTTP a ser usado. Para obter informações sobre verbos disponíveis, consulte [definições de método](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)