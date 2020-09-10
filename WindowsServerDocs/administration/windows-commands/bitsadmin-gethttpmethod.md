---
title: bitsadmin gethttpmethod
description: Artigo de referência para o comando Bitsadmin gethttpmethod, que obtém o verbo HTTP a ser usado com o trabalho.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 0d96c85aa99330b133954a77ca5584fe2d02cd43
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632034"
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
