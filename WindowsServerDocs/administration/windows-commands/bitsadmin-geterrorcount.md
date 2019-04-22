---
title: bitsadmin geterrorcount
description: Tópico de comandos do Windows para **geterrorcount bitsadmin** -recupera uma contagem do número de vezes que o trabalho especificado gerado um erro transitório.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91045372931efec0e3189132a275eeacab584de4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818367"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount



Recupera uma contagem do número de vezes que o trabalho especificado gerado um erro transitório.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetErrorCount <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera informações de contagem de erro do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetErrorCount myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)