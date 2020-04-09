---
title: bitsadmin setnotifyflags
description: Tópico de comandos do Windows para Bitsadmin setnotifyflags, que define os sinalizadores de notificação de eventos para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd3001fa4ae7f51cab92556f4f2f498511cca5ae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849279"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Define os sinalizadores de notificação de eventos para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|NotifyFlags|Ver comentários|

## <a name="remarks"></a>Comentários

O parâmetro **NotifyFlags** pode conter um ou mais dos seguintes sinalizadores de notificação.

|-----|-----| | 1 | Gerar um evento quando todos os arquivos no trabalho forem transferidos. | | 2 | Gerar um evento quando ocorrer um erro. | | 4 | Desabilitar notificações. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define a tarefa notificar sinalizadores para o trabalho de eventos de erro e transferidos para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)