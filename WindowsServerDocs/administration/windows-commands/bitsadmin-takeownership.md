---
title: bitsadmin takeownership
description: Tópico de comandos do Windows para **Bitsadmin TakeOwnership** – permite que um usuário com privilégios administrativos assuma a propriedade do trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0d0610b2ba6437f6fdd41bf1b875993cf11f2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380360"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership



Permite que um usuário com privilégios administrativos assuma a propriedade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /TakeOwnership <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir assume a propriedade do trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)