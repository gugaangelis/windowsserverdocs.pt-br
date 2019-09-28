---
title: bitsadmin getnotifyinterface
description: Tópico de comandos do Windows para **Bitsadmin getnotifyinterface** – determina se outro programa registrou uma interface de retorno de chamada com para o trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 826e13cf8a3e54935ceb5a72ff82647cacfc3be5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381469"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Determina se outro programa registrou uma interface de retorno de chamada COM (a interface de notificação) para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetNotifyInterface <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Exibe registrado ou não registrado.

> [!NOTE]
> Não é possível determinar o programa que registrou a interface de retorno de chamada.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera a interface de notificação para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyInterface myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)