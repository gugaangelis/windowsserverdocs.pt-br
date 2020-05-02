---
title: bitsadmin setpeercachingflags
description: Tópico de referência para o comando Bitsadmin setpeercachingflags, que define sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os colegas e se o trabalho pode baixar conteúdo de pares.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b66b169c38ac050ecaaf6546365547148faa9cf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717268"
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
