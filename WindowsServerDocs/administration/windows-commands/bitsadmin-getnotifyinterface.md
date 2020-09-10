---
title: bitsadmin getnotifyinterface
description: Artigo de referência para o comando Bitsadmin getnotifyinterface, que determina se outro programa registrou uma interface de retorno de chamada COM para o trabalho especificado.
ms.topic: reference
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 563cdf103ce60fc13f0455caea98e30f9e2e03e4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631897"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Determina se outro programa registrou uma interface de retorno de chamada COM (a interface de notificação) para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getnotifyinterface <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

#### <a name="output"></a>Saída

A saída para esse comando exibe, **registrado** ou não **registrado**.

> [!NOTE]
> Não é possível determinar o programa que registrou a interface de retorno de chamada.

## <a name="examples"></a>Exemplos

Para recuperar a interface de notificação para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
