---
title: bitsadmin complete
description: Tópico de comandos do Windows para **bitsadmin completa** -conclui o trabalho. Os arquivos baixados não estão disponíveis para você até que você use essa opção.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 561585da370f7e69aa3b83b3ddd7579bfc658a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817317"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Conclui o trabalho. Os arquivos baixados não estão disponíveis para você até que você use essa opção. Use essa opção depois que o trabalho passa para o estado transferido. Caso contrário, somente os arquivos que foram transferidos com êxito estão disponíveis.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

Quando o estado do trabalho é transferido, o BITS foi transferida com êxito todos os arquivos no trabalho. No entanto, os arquivos não estão disponíveis até que você use o **/ completo** alternar. Se usam vários trabalhos *myDownloadJob* como seu nome, você deve substituir *myDownloadJob* com o GUID do trabalho para identificar exclusivamente o trabalho.
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)