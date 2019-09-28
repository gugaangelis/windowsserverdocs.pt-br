---
title: diskperf
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 829f0284d761e6a5134011fa1dff99646d55fc13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377815"
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
|-Y|Inicie todos os contadores de desempenho de disco quando o computador for reiniciado.|
|-YD|Habilitar contadores de desempenho de disco para unidades físicas quando o computador for reiniciado.|
|-YV|Habilite os contadores de desempenho de disco para unidades lógicas ou volumes de armazenamento quando o computador for reiniciado.|
|-N|Desabilite todos os contadores de desempenho de disco quando o computador for reiniciado.|
|-ND|Desabilite os contadores de desempenho de disco para unidades físicas quando o computador for reiniciado.|
|-NV|Desabilite os contadores de desempenho de disco para unidades lógicas ou volumes de armazenamento quando o computador for reiniciado.|
|\\ @ no__t-1 *\<computername >*|Especifique o nome do computador no qual você deseja habilitar ou desabilitar os contadores de desempenho de disco.|