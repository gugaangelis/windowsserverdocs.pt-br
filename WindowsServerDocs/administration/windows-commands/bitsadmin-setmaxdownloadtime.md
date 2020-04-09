---
title: bitsadmin setmaxdownloadtime
description: Tópico de comandos do Windows para Bitsadmin setmaxdownloadtime, que define o tempo limite de download em segundos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd44011cd14d575a9c3798ede45641fac4c3dc75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849369"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime

Define o tempo limite de download em segundos.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Limite de tempo|O tempo limite em segundos|

## <a name="remarks"></a>Comentários

-   {1&gt;N/A&lt;1}

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define o tempo limite para o trabalho chamado *myDownloadJob* como 10 segundos.
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)