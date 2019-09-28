---
title: bitsadmin complete
description: O tópico de comandos do Windows para **Bitsadmin Complete** -conclui o trabalho. Os arquivos baixados não estarão disponíveis até você até que você use essa opção.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d5a1dc5dbbf2d5b3207b5423f338e0caf4412599
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381822"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Conclui o trabalho. Os arquivos baixados não estarão disponíveis até você até que você use essa opção. Use essa opção depois que o trabalho for movido para o estado transferido. Caso contrário, somente os arquivos que foram transferidos com êxito estarão disponíveis.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

Quando o estado do trabalho for transferido, o BITS transferiu com êxito todos os arquivos no trabalho. No entanto, os arquivos não estarão disponíveis até que você use a opção **/Complete** Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá substituir *myDownloadJob* pelo GUID do trabalho para identificar exclusivamente o trabalho.
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)