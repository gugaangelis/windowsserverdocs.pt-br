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
ms.openlocfilehash: 97d889b54c4b0de0230c50e1d7c8d21617ea881a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835847"
---
#<a name="bitsadmin-getsecurityflags"></a>Bitsadmin getsecurityflags

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
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)


