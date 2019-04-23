---
title: bitsadmin getaclflags
description: Tópico de comandos do Windows para **getaclflags bitsadmin** -recupera os sinalizadores de propagação de lista de controle de acesso.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 185445a97168344f910abc0e644718296de2c712
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861447"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera os sinalizadores de propagação de ACL (lista) de controle de acesso.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Exibe um ou mais dos seguintes valores de sinalizador:
-   O: Copie informações do proprietário com o arquivo.
-   G: Copie informações de grupo com o arquivo.
-   D: Copie informações da DACL com o arquivo.
-   S: Copie informações da SACL com o arquivo.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera os sinalizadores de propagação de lista de controle do acesso do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)