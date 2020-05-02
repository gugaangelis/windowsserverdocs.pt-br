---
title: bitsadmin getvalidationstate
description: Tópico de referência para o comando getvalidable Bitsadmin, que relata o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca753b20a1b7834d2e05d4ff8729a08332256f8c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717456"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate

Relata o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getvalidationstate <job> <file_index>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| file_index | Começa em 0. |

## <a name="examples"></a>Exemplos

Para recuperar o estado de validação de conteúdo do arquivo 2 no trabalho chamado *myDownloadJob*:

```
bitsadmin /getvalidationstate myDownloadJob 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
