---
title: bitsadmin removeclientcertificate
description: Tópico de comandos do Windows para **Bitsadmin removeclientcertificate** – remove o certificado do cliente do trabalho.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c664ba9b26f3511dedf35477a1cd393db709337e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381036"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate



Remove o certificado do cliente do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /RemoveClientCertificate <Job> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir remove o certificado do cliente do trabalho chamado *myJob*.
```
C:\>Bitsadmin /RemoveClientCertificate myJob 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)