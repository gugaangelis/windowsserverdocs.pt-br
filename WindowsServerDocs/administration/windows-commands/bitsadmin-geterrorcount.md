---
title: bitsadmin geterrorcount
description: Artigo de referência para o comando Bitsadmin GetErrorCount, que recupera uma contagem do número de vezes que o trabalho especificado gerou um erro transitório.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90eaa150f2decba4bbee693ac117cd269d5a7c97
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923050"
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
