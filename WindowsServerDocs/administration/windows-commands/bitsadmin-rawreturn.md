---
title: bitsadmin rawreturn
description: Tópico de comandos do Windows para **rawreturn bitsadmin** -retorna dados adequado para análise.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4e12c8e621021d35ac618b4592515fe38c36be0e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434896"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retorna dados adequado para análise.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>Comentários

Caracteres de nova linha de faixas e a formatação da saída.

Em geral, use este comando em conjunto com o **Create** e **obter\\** * comutadores para receber apenas o valor. Você deve especificar essa opção antes de outras opções.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera os dados brutos para o estado do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)