---
title: bitsadmin getdisplayname
description: Tópico de comandos do Windows para **bitsadmin getdisplayname** -recupera o nome de exibição do trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1ef16f54b7b825e4293a3870d8181985b83843b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857597"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname



Recupera o nome de exibição do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetDisplayName <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o nome de exibição do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)