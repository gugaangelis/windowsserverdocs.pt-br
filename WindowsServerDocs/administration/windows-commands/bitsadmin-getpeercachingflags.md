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
ms.openlocfilehash: eabcc5cacdcc5f7f4de7178b5afeff2acc89d7a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862807"
---
#<a name="bitsadmin-getpeercachingflags"></a>Bitsadmin getpeercachingflags

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
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)


