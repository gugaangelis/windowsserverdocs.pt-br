---
title: bitsadmin getsecurityflags
description: O tópico de comandos do Windows para **Bitsadmin getsecurityflags** -relata os sinalizadores de segurança http para o redirecionamento de URL e as verificações executadas no certificado do servidor durante a transferência.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb53664a6366b411ae1eb9b0fe7c93392d60b542
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381464"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Relata os sinalizadores de segurança HTTP para o redirecionamento de URL e as verificações executadas no certificado do servidor durante a transferência.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso
O exemplo a seguir recupera os sinalizadores securitly de um trabalho chamado *myJob*.

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)


