---
title: versão e bitsadmin util
description: Tópico de comandos do Windows para **util bitsadmin e versão** -exibe a versão do serviço do BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e768ec5ae43fc17c480b9deede698cca01c6291
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882867"
---
# <a name="bitsadmin-util-and-version"></a>versão e bitsadmin util

Exibe a versão do serviço de BITS (por exemplo, 2.0).

**BITSAdmin 1.5 e anterior**: Sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Comentários

O **Verbose** switch executará o seguinte:
-   Exibe a versão do arquivo para cada DLL relacionada de BITS
-   Verifica que o serviço BITS pode ser iniciado
-   Exibe os valores de diretiva de grupo do BITS (apenas no Windows Vista)

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir a versão do serviço do BITS.
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)