---
title: Bitsadmin de cache e getconfigurationflags
description: O tópico de comandos do Windows para **Bitsadmin de cache e getconfigurationflags** -Obtém os sinalizadores de configuração que determinam se o computador fornece conteúdo a pares e pode baixar conteúdo de pares.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 94c7eb1a115fe9152b149b8cf65765b179080cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381094"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>Bitsadmin de cache e getconfigurationflags



Obtém os sinalizadores de configuração que determinam se o computador fornece conteúdo a pares e pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir obtém os sinalizadores de configuração para o trabalho chamado *myJob*.
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)