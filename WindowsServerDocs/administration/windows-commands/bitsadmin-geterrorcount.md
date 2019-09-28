---
title: bitsadmin geterrorcount
description: Tópico de comandos do Windows para **Bitsadmin GetErrorCount** – recupera uma contagem do número de vezes que o trabalho especificado gerou um erro transitório.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e5aa64c0e080e946e84c0bf804527bb00cad70a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381624"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount



Recupera uma contagem do número de vezes que o trabalho especificado gerou um erro transitório.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetErrorCount <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera informações de contagem de erros para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetErrorCount myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)