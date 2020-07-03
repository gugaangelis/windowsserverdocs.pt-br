---
title: bitsadmin setnotifyflags
description: Artigo de referência para o comando Bitsadmin setnotifyflags, que define os sinalizadores de notificação de eventos para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4054788bb8c14e4bd9be38296f5c0f933de9462
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927618"
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
