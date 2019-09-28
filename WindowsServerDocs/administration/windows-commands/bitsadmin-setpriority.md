---
title: bitsadmin setpriority
description: O tópico de comandos do Windows para **Bitsadmin setanteriority** – define a prioridade do trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60564350928f917ca1861684e042304d5d380426
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380442"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



Define a prioridade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Priority|Um dos seguintes valores:</br>-PRIMEIRO PLANO</br>-ALTA</br>-NORMAL</br>-BAIXO|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir define a prioridade para o trabalho chamado *myDownloadJob* como normal.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)