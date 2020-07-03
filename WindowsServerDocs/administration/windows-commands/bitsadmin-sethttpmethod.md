---
title: bitsadmin sethttpmethod
description: Artigo de referência para o comando Bitsadmin sethttpmethod, que define o verbo HTTP a ser usado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 86d4749de294871a05176239cc1265974d8a7590
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927756"
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

- [comando Bitsadmin](bitsadmin.md)
