---
title: bitsadmin getaclflags
description: O tópico de comandos do Windows para **Bitsadmin getaclflags** -recupera os sinalizadores de propagações da lista de controle de acesso.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ad98cd742161ae06be5cba7acde7b810eaf199d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381792"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera os sinalizadores de propagações da ACL (lista de controle de acesso).

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Exibe um ou mais dos seguintes valores de sinalizador:
-   MINÚSCULA Copie as informações do proprietário com o arquivo.
-   M Copie as informações do grupo com o arquivo.
-   D: Copie informações de DACL com o arquivo.
-   & Copie as informações da SACL com o arquivo.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera os sinalizadores de propagação da lista de controle de acesso para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)