---
title: bitsadmin setnotifyflags
description: Artigo de referência para o comando Bitsadmin setnotifyflags, que define os sinalizadores de notificação de eventos para o trabalho especificado.
ms.topic: reference
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 48eb165ecb32e53868f9636418437d66c56542c0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630775"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Define os sinalizadores de notificação de eventos para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setnotifyflags <job> <notifyflags>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| notifyflags | Pode incluir um ou mais dos seguintes sinalizadores de notificação, incluindo:<ul><li>**1.** gera um evento quando todos os arquivos no trabalho são transferidos.</li><li>**2.** gera um evento quando ocorre um erro.</li><li>**3.** gera um evento quando todos os arquivos têm a transferência concluída ou quando ocorre um erro.</li><li>**4.** desabilita as notificações.</li></ul> |

## <a name="examples"></a>Exemplos

Para definir os sinalizadores de notificação para gerar um evento quando ocorrer um erro, para um trabalho chamado *myDownloadJob*:

```
bitsadmin /setnotifyflags myDownloadJob 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
