---
title: bitsadmin geterrorcount
description: Artigo de referência para o comando Bitsadmin GetErrorCount, que recupera uma contagem do número de vezes que o trabalho especificado gerou um erro transitório.
ms.topic: reference
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b6a4d3f0e77d8ffc0d7e538affe0fb8e77a5281
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030344"
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
