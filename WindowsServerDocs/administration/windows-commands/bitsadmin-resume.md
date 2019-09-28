---
title: bitsadmin resume
description: O tópico de comandos do Windows para **Bitsadmin resume** -ativa um trabalho novo ou suspenso na fila de transferência.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1393e959980b72de09c546ced763a506d334b56c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380774"
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
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir retoma o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /Resume myDownloadJob
```
Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)