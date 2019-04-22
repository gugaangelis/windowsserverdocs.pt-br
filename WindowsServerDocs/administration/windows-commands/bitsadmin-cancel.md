---
title: bitsadmin cancel
description: Tópico de comandos do Windows para **bitsadmin Cancelar** - remove o trabalho da fila de transferência e exclui todos os arquivos temporários associados ao trabalho.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a4d1e2d6e4fd66cb525316f236d070fcd72d73f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814067"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Remove o trabalho da fila de transferência e exclui todos os arquivos temporários associados ao trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cancel <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir remove a *myDownloadJob* trabalho da fila de transferência.
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)