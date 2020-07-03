---
title: bitsadmin gethttpmethod
description: Artigo de referência para o comando Bitsadmin gethttpmethod, que obtém o verbo HTTP a ser usado com o trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 3a963eb275c6e635849094906c52fedf90dccd0d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927028"
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
