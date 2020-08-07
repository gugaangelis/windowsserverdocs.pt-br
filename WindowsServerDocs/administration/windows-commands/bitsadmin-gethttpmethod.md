---
title: bitsadmin gethttpmethod
description: Artigo de referência para o comando Bitsadmin gethttpmethod, que obtém o verbo HTTP a ser usado com o trabalho.
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: ea6f1b3896b11bfcc157ea54dc0470786d3dff1f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894223"
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
