---
title: bitsadmin setnotifyflags
description: Tópico de comandos do Windows para **Bitsadmin setnotifyflags**, que define os sinalizadores de notificação de eventos para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73c088ce2bae8d2ad99b313417c14449ddd822b5
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122795"
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

O exemplo a seguir define os sinalizadores de notificação para gerar um evento quando ocorre um erro, para um trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /setnotifyflags myDownloadJob 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)