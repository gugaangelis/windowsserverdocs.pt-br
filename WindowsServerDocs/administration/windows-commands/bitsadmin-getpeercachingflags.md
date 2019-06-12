---
title: Bitsadmin getpeercachingflags
description: Tópico de comandos do Windows para **getpeercachingflags bitsadmin** -recupera sinalizadores que determinam se os arquivos de trabalho podem ser armazenados em cache e atendidos para pares, e se o BITS podem baixar o conteúdo para o trabalho de pares.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 28f248bab3e3cc3f5c7dd4f5f878f0b6d776029b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434918"
---
# <a name="bitsadmin-getpeercachingflags"></a>Bitsadmin getpeercachingflags

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera os sinalizadores que determinam se os arquivos de trabalho podem ser armazenados em cache e atendidos para pares, e se o BITS podem baixar o conteúdo para o trabalho de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos
O exemplo a seguir recupera os sinalizadores do trabalho nomeado *myJob*.

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)


