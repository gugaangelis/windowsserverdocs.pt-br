---
title: bitsadmin setminretrydelay
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 640492cf690a934e3e3b8d0ecf8ca7a0d6a7dc2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813077"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Define o comprimento mínimo de tempo, em segundos, que BITS espera depois de encontrar um erro transitório antes de tentar transferir o arquivo.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|RetryDelay|Um número representado em segundos.|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define o atraso de repetição mínima do trabalho nomeado *myDownloadJob* para 35 segundos.
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)