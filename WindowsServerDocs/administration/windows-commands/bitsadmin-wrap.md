---
title: bitsadmin wrap
description: Tópico de comandos do Windows para **wrap bitsadmin** -encapsula qualquer linha de texto de saída estender além da borda mais à direita da janela de comando para a próxima linha.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4834a8a17c72394b6ee8f051ec76919af9880124
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881667"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Encapsula o resultado para caber em uma janela de comando.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Especifique antes de outras opções. Por padrão, todos os comutadores, exceto o [bitsadmin monitor](bitsadmin-monitor.md) alternar, encapsule a saída.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera informações do trabalho nomeado *myDownloadJob* e encapsula a saída.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
