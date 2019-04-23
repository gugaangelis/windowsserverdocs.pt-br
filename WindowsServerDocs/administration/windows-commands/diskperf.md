---
title: diskperf
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a99f18b56c9295e902a3c89e2e89b36c9c1b6c89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852577"
---
# <a name="diskperf"></a>diskperf



No Windows 2000, os contadores de desempenho de disco físico e lógico não estão habilitados por padrão.

**Diskperf** está incluído no Windows XP, Windows Server 2003, Windows Server 2008, Windows Vista, Windows Server 2008 R2 e Windows 7 para que ele pode ser usado para habilitar ou desabilitar os contadores de desempenho de disco lógico ou físico em computadores que executam remotamente Windows 2000.

## <a name="syntax"></a>Sintaxe

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>Opções

|Opção|Descrição|
|------|-----------|
|-?|Sensíveis ao contexto de exibe a Ajuda.|
|-Y|Inicie todos os contadores de desempenho de disco quando o computador for reiniciado.|
|-YD|Habilite os contadores de desempenho de disco para unidades físicas quando o computador for reiniciado.|
|-YV|Habilite os contadores de desempenho de disco para as unidades lógicas ou volumes de armazenamento quando o computador for reiniciado.|
|-N|Desabilite todos os contadores de desempenho de disco quando o computador for reiniciado.|
|-ND|Desabilite os contadores de desempenho de disco para unidades físicas quando o computador for reiniciado.|
|-NV|Desabilite os contadores de desempenho de disco para volumes de armazenamento ou unidades lógicas quando o computador for reiniciado.|
|\\\\*\<computername>*|Especifique o nome do computador no qual você deseja habilitar ou desabilitar os contadores de desempenho de disco.|