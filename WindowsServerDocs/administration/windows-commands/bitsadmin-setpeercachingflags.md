---
title: bitsadmin setpeercachingflags
description: Artigo de referência para o comando Bitsadmin setpeercachingflags, que define sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os colegas e se o trabalho pode baixar conteúdo de pares.
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73135ed6f08962cbe73aad4765dc4d3b4f7ff958
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892976"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

Define sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os colegas e se o trabalho pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setpeercachingflags <job> <value>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| value | Um inteiro sem sinal, incluindo:<ul><li>**1.** o trabalho pode baixar conteúdo de pares.</li><li>**2.** os arquivos do trabalho podem ser armazenados em cache e servidos para os pares.</li></ul> |

## <a name="examples"></a>Exemplos

Para permitir que o trabalho chamado *myDownloadJob* Baixe o conteúdo dos pares:

```
bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
