---
title: bitsadmin sethelpertoken
description: O tópico de comandos do Windows para **Bitsadmin sethelpertoken** -define o token primário do prompt de comando atual (ou um token de conta de usuário local arbitrário, se especificado) como um token auxiliar do trabalho de transferência de bits.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 91c03366998168dad9ab4530ef36a5020b8ad6ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380572"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

Define o token primário do prompt de comando atual (ou um token de conta de usuário local arbitrário, se especificado) como um [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)do trabalho de transferência de bits.

**BITS 3,0 e anteriores**: sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho.|
|\<username@domain\> \<senha\>|Opcional&mdash;as credenciais de uma conta de usuário local cujo token deve ser usado.|

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
