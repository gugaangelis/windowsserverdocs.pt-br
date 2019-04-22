---
title: getconfigurationflags e cache de par bitsadmin
description: Tópico de comandos do Windows para **cache de par bitsadmin e getconfigurationflags** - obtém os sinalizadores de configuração que determinam se o computador serve o conteúdo para pontos e pode baixar conteúdo de pares.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6afa39993cf90b2d71b6b681680c3b4e1fd9b56b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826347"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>getconfigurationflags e cache de par bitsadmin



Obtém os sinalizadores de configuração que determinam se o computador serve um conteúdo para pontos e pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir obtém os sinalizadores de configuração do trabalho nomeado *myJob*.
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)