---
title: bitsadmin getnotifyinterface
description: Artigo de referência para o comando Bitsadmin getnotifyinterface, que determina se outro programa registrou uma interface de retorno de chamada COM para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c86b611eb5f46759c474171085884c5702e07d1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926908"
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
