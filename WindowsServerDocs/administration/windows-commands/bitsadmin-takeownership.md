---
title: bitsadmin takeownership
description: Tópico de referência para o comando Bitsadmin TakeOwnership, que permite que um usuário com privilégios administrativos assuma a propriedade do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5369cb3fa143ebde77ae8cabf04b9a38eed5b9c5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720438"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Permite que um usuário com privilégios administrativos assuma a propriedade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /takeownership <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ---------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para apropriar-se do trabalho chamado *myDownloadJob*:

```
bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
