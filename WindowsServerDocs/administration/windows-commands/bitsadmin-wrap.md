---
title: bitsadmin wrap
description: O tópico de comandos do Windows para **Bitsadmin Wrap** -quebra qualquer linha de texto de saída que se estende além da borda mais à direita da janela de comando para a próxima linha.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5609fb6f38716795a545e0c7fe3939f893a8c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380681"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Delimita a saída para caber em uma janela de comando.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Especifique antes de outras opções. Por padrão, todas as opções, exceto a opção de [Monitor Bitsadmin](bitsadmin-monitor.md) , encapsulam a saída.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera informações para o trabalho chamado *myDownloadJob* e encapsula a saída.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
