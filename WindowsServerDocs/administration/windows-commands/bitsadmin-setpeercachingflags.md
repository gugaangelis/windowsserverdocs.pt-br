---
title: Bitsadmin setpeercachingflags
description: Tópico de comandos do Windows para **setpeercachingflags bitsadmin** -define os sinalizadores que determinam se os arquivos de trabalho podem ser armazenados em cache e servia para colegas e se o trabalho pode baixar o conteúdo de pares.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2d50a6ccd83a6251808ca3d66437e52f641c60a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814247"
---
# <a name="bitsadmin-setpeercachingflags"></a>Bitsadmin setpeercachingflags



Define os sinalizadores que determinam se os arquivos de trabalho podem ser armazenados em cache e servia para colegas e se o trabalho pode baixar conteúdo de colegas.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Valor|O valor é um inteiro sem sinal com a seguinte interpretação para os bits na representação binária.</br>1 - o trabalho pode baixar o conteúdo de pares.</br>2 - os arquivos do trabalho podem ser armazenado em cache e servia para colegas.|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define os sinalizadores do trabalho nomeado *myJob* que permite que ele baixar conteúdo de pares.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)