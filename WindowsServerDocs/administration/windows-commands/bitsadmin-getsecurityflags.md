---
title: Bitsadmin getsecurityflags
description: Tópico de comandos do Windows para **getsecurityflags bitsadmin** - reporta os sinalizadores de segurança HTTP para redirecionamento de URL e verificações realizadas no certificado do servidor durante a transferência.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e1db167b12d47afccb8842da617f1e9fe72acff
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434966"
---
# <a name="bitsadmin-getsecurityflags"></a>Bitsadmin getsecurityflags

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sinalizadores de segurança de relatórios HTTP para redirecionamento de URL e verificações executadas no certificado do servidor durante a transferência.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos
O exemplo a seguir recupera os sinalizadores de securitly de um trabalho denominado *myJob*.

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)


