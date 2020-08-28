---
title: bitsadmin gethttpmethod
description: Artigo de referência para o comando Bitsadmin gethttpmethod, que obtém o verbo HTTP a ser usado com o trabalho.
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 5bbd2509cabea7ae68240a31cee78d9ffd3ac5e0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033524"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

Obtém o verbo HTTP a ser usado com o trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o verbo HTTP a ser usado com o trabalho chamado *myDownloadJob*:

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
