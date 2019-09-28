---
title: bitsadmin getnoprogresstimeout
description: Tópico de comandos do Windows para **Bitsadmin getnoprogresstimeout** – recupera o período de tempo, em segundos, que o serviço tenta transferir o arquivo depois que um erro transitório ocorre.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7dcc0e445f4cae25c27f5ff70c73f4f2f23975aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381497"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout



Recupera o período de tempo, em segundos, que o serviço tenta transferir o arquivo após ocorrer um erro transitório.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetNoProgressTimeout <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o valor de tempo limite de progresso para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetNoProgressTimeout myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)