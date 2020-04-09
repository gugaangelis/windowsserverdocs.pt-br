---
title: utilitário e versão do Bitsadmin
description: Tópico de comandos do Windows para o Bitsadmin util e Version, que exibe a versão do serviço BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 087cc1033166ab93e7496caaa7335433cafd6249
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848829"
---
# <a name="bitsadmin-util-and-version"></a>utilitário e versão do Bitsadmin

Exibe a versão do serviço BITS (por exemplo, 2,0).

**BITSAdmin 1,5 e anterior**: sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Comentários

O comutador **detalhado** executa o seguinte:
-   Exibe a versão do arquivo para cada DLL relacionada ao BITS
-   Verifica se o serviço BITS pode ser iniciado
-   Exibe valores Política de Grupo BITS (somente Windows Vista)

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir a versão do serviço BITS.
```
C:\>bitsadmin /Util /Version
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)