---
title: bitsadmin setminretrydelay
description: O tópico de comandos do Windows para Bitsadmin setminretrydelay, que define o período mínimo de tempo, em segundos, que o BITS aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb2fe4c6d0e4f90c6ec49fa1da63404393d4f634
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849359"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Define o período mínimo de tempo, em segundos, que o BITS aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|RetryDelay|Um número representado em segundos.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define o atraso mínimo de repetição para o trabalho chamado *myDownloadJob* a 35 segundos.
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)