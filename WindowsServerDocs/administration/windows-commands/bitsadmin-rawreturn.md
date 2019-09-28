---
title: bitsadmin rawreturn
description: O tópico de comandos do Windows para **Bitsadmin rawreturn** -retorna dados adequados para análise.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86d769de460538acda696194348980de5752d6d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380881"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retorna os dados adequados para análise.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>Comentários

Remove os caracteres de nova linha e a formatação da saída.

Normalmente, você usa esse comando em conjunto com as opções **criar** e **obter @ no__t-2*** para receber apenas o valor. Você deve especificar essa opção antes de outras opções.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera os dados brutos para o estado do trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)