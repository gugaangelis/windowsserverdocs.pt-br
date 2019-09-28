---
title: bitsadmin getminretrydelay
description: Tópico de comandos do Windows para **Bitsadmin getminretrydelay** – recupera o período de tempo, em segundos, que o serviço aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a2bde6340034e48b97b4c86f48a3b2ef72560a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381555"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay



Recupera o período de tempo, em segundos, que o serviço aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetMinRetryDelay <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o atraso mínimo de repetição para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetMinRetryDelay myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)