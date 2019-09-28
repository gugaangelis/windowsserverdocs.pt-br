---
title: utilitário e versão do Bitsadmin
description: Tópico de comandos do Windows para o **Bitsadmin util e Version** – exibe a versão do serviço bits.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 495ef17bbf6f39f20f6729b64de4b4bec0f9a3c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380202"
---
# <a name="bitsadmin-util-and-version"></a>utilitário e versão do Bitsadmin

Exibe a versão do serviço BITS (por exemplo, 2,0).

**BITSAdmin 1,5 e anterior**: Não compatível.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Comentários

O comutador **detalhado** executa o seguinte:
-   Exibe a versão do arquivo para cada DLL relacionada ao BITS
-   Verifica se o serviço BITS pode ser iniciado
-   Exibe valores Política de Grupo BITS (somente Windows Vista)

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir a versão do serviço BITS.
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)