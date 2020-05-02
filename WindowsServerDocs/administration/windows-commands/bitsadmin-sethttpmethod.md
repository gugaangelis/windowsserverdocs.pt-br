---
title: bitsadmin sethttpmethod
description: Tópico de referência para o comando Bitsadmin sethttpmethod, que define o verbo HTTP a ser usado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: daf1c23565bc4f398fd29e51aaaeef23b3b0d018
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719356"
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
