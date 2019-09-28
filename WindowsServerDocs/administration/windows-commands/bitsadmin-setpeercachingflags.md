---
title: bitsadmin setpeercachingflags
description: O tópico de comandos do Windows para **Bitsadmin setpeercachingflags** -define sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os colegas e se o trabalho pode baixar conteúdo de pares.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 147f28268f1b4dd6dfb40cff85f073feabbc35a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380457"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags



Define sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os colegas e se o trabalho pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Valor|O valor é um inteiro não assinado com a seguinte interpretação para os bits na representação binária.</br>1-o trabalho pode baixar o conteúdo de pares.</br>2-os arquivos do trabalho podem ser armazenados em cache e servidos para os pares.|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir define sinalizadores para o trabalho chamado *myJob* , que permite que ele baixe o conteúdo de pares.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)