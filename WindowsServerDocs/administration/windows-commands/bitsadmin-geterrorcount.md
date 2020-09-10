---
title: bitsadmin geterrorcount
description: Artigo de referência para o comando Bitsadmin GetErrorCount, que recupera uma contagem do número de vezes que o trabalho especificado gerou um erro transitório.
ms.topic: reference
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7266af9244218cf4a6434c838390eac8149c80ab
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632111"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount

Recupera uma contagem do número de vezes que o trabalho especificado gerou um erro transitório.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /geterrorcount <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar informações de contagem de erros para o trabalho chamado *myDownloadJob*:

```
bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
