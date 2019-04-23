---
title: bitsadmin setnotifyflags
description: Tópico de comandos do Windows para **setnotifyflags bitsadmin** -define o evento de sinalizadores de notificação para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bc817e03e0f1916ea392830d14985a7a1377d69a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868787"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Define o evento de sinalizadores de notificação para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|NotifyFlags|Consulte os comentários|

## <a name="remarks"></a>Comentários

O **NotifyFlags** parâmetro pode conter um ou mais dos seguintes sinalizadores de notificação.

|---|---| | 1 | Gerar um evento quando o trabalho de todos os arquivos foram transferidos. | | 2 | Gerar um evento quando ocorre um erro. | | 4 | Desabilitar as notificações. |

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define os sinalizadores de notificação para transferida e eventos de erro do trabalho de trabalho denominado *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)