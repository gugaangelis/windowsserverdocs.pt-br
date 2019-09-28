---
title: bitsadmin getclientcertificate
description: Tópico de comandos do Windows para **Bitsadmin GetClientCertificate** – recupera o certificado do cliente do trabalho.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 613feafb442f63513d34e9038647c4dbeb278630
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381720"
---
# <a name="bitsadmin-getclientcertificate"></a>bitsadmin getclientcertificate



Recupera o certificado do cliente do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetClientCertificate <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o certificado do cliente para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin / GetClientCertificate myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)