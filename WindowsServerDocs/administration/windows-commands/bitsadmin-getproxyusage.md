---
title: bitsadmin getproxyusage
description: Tópico de comandos do Windows para **getproxyusage bitsadmin** -recupera a configuração de uso de proxy para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 20ba418b8dfcf3d96d9b20b22e53797a232a13f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863877"
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
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Os valores possíveis são:
-   Pré-configurar — use os padrões do Internet Explorer do proprietário.
-   NO_PROXY — não use um servidor proxy.
-   Substituir – Use uma lista de proxy explícito.
-   Detecção automática — detectar automaticamente as configurações de proxy.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o uso do proxy do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)