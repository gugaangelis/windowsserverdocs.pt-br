---
title: bitsadmin getcreationtime
description: Artigo de referência para o comando Bitsadmin GetCreationTime, que recupera a hora de criação para o trabalho especificado.
ms.topic: reference
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1175b148e0169f5b8f76d66ae3358a1069f5c4f7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030424"
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
