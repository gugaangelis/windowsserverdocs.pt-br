---
title: bitsadmin resume
description: Tópico de comandos do Windows para **bitsadmin retomar** -ativa um trabalho novo ou suspenso na fila de transferência.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76027ac927f8a9bb2558e3ce6d75e4f6692e56e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842027"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume



Ativa um trabalho novo ou suspenso na fila de transferência.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Resume <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir retoma o trabalho denominado *myDownloadJob*.
```
C:\>bitsadmin /Resume myDownloadJob
```
Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)