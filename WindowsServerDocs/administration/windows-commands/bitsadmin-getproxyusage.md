---
title: bitsadmin getproxyusage
description: Tópico de comandos do Windows para **Bitsadmin getproxyusage** – recupera a configuração de uso de proxy para o trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea9a22f4fb35af3436d02d9f23b62ce0888a26b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381294"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



Recupera a configuração de uso de proxy para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetProxyUsage <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Os valores possíveis são:
-   Preconfig – use os padrões do Internet Explorer do proprietário.
-   NO_PROXY — não use um servidor proxy.
-   SUBSTITUIR — use uma lista de proxy explícita.
-   DETECÇÃO automática — detecta automaticamente as configurações de proxy.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o uso de proxy para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)