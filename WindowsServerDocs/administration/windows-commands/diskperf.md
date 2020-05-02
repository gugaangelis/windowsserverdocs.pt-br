---
title: diskperf
description: Tópico de referência para diskperf, que pode ser usado para habilitar ou desabilitar remotamente os contadores de desempenho de disco físico ou lógico em computadores que executam o Windows 2000.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8f505d924ee1de311f2f2736ff65be844c3f2ea
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719443"
---
# <a name="diskperf"></a>diskperf

No Windows 2000, os contadores de desempenho de disco físico e lógico não estão habilitados por padrão.

O **diskperf** está incluído no Windows XP, no windows Server 2003, no windows Server 2008, no Windows Vista, no windows Server 2008 R2 e no Windows 7 para que possa ser usado para habilitar ou desabilitar remotamente os contadores de desempenho de disco físico ou lógico em computadores que executam o Windows 2000.

## <a name="syntax"></a>Sintaxe

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>Opções

|Opção|Descrição|
|------|-----------|
|-?|Exibe a ajuda contextual.|
|-y|Inicie todos os contadores de desempenho de disco quando o computador for reiniciado.|
|-YD|Habilitar contadores de desempenho de disco para unidades físicas quando o computador for reiniciado.|
|-YV|Habilite os contadores de desempenho de disco para unidades lógicas ou volumes de armazenamento quando o computador for reiniciado.|
|-n|Desabilite todos os contadores de desempenho de disco quando o computador for reiniciado.|
|-ND|Desabilite os contadores de desempenho de disco para unidades físicas quando o computador for reiniciado.|
|-NV|Desabilite os contadores de desempenho de disco para unidades lógicas ou volumes de armazenamento quando o computador for reiniciado.|
|\\\\*\<ComputerName>*|Especifique o nome do computador no qual você deseja habilitar ou desabilitar os contadores de desempenho de disco.|