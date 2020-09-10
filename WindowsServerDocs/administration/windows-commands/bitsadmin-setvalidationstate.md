---
title: bitsadmin setvalidationstate
description: Artigo de referência do comando setvalidable Bitsadmin, que define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.
ms.topic: reference
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1f30807e392ede7710c4d7740416d2cdb34c1378
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630612"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate

Define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setvalidationstate <job> <file_index> <TRUE|FALSE>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ---------- |
| Trabalho | O nome de exibição ou o GUID do trabalho. |
| file_index | Começa em 0. |
| TRUE ou FALSE | **True** ativa a validação de conteúdo para o arquivo especificado, enquanto **false** o desativa. |

## <a name="examples"></a>Exemplos

Para definir o estado de validação de conteúdo do arquivo 2 como TRUE para o trabalho chamado *myDownloadJob*:

```
bitsadmin /setvalidationstate myDownloadJob 2 TRUE
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
