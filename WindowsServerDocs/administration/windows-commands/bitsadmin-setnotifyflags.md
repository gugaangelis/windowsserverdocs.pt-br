---
title: bitsadmin setnotifyflags
description: O tópico de comandos do Windows para **Bitsadmin setnotifyflags** -define os sinalizadores de notificação de eventos para o trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9cfabf05610cbbe8fa65fd16b0d33e161dcef9b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380455"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Define os sinalizadores de notificação de eventos para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|NotifyFlags|Ver comentários|

## <a name="remarks"></a>Comentários

O parâmetro **NotifyFlags** pode conter um ou mais dos seguintes sinalizadores de notificação.

|-----|-----| | 1 | Gerar um evento quando todos os arquivos no trabalho forem transferidos. | | 2 | Gerar um evento quando ocorrer um erro. | | 4 | Desabilitar notificações. |

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir define a tarefa notificar sinalizadores para o trabalho de eventos de erro e transferidos para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)