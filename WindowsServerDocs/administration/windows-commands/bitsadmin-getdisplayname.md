---
title: bitsadmin getdisplayname
description: O tópico de comandos do Windows para **Bitsadmin GetDisplayName** – recupera o nome de exibição do trabalho especificado.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 229bd245f9e810fc6aeb856bbfba253b9ab8a9f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381632"
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
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o nome de exibição para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)