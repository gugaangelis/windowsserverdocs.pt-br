---
title: bitsadmin getcreationtime
description: Artigo de referência para o comando Bitsadmin GetCreationTime, que recupera a hora de criação para o trabalho especificado.
ms.topic: reference
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 780f7124248bd38f0c3d7cc9e7cb14b1a9decc03
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632230"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

Recupera a hora de criação para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a hora de criação para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
