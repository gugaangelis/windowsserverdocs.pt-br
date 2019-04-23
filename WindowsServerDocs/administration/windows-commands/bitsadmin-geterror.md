---
title: bitsadmin geterror
description: Tópico de comandos do Windows para **geterror bitsadmin** -recupera informações detalhadas de erros para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10a3373c0c8f290ff1f5f26ef38531fbc7745890
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889967"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror



Recupera informações de erro detalhadas para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetError <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera as informações de erro do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetError myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)