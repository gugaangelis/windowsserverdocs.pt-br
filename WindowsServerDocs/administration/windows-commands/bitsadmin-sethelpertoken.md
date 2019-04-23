---
title: Bitsadmin sethelpertoken
description: Tópico de comandos do Windows para **sethelpertoken bitsadmin** -define de token primário do prompt de comando atual (ou token da conta de usuário local arbitrário, se especificado) como um token do auxiliar de um trabalho transferência do BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 558a1aca66a7b3ec447136ceff9237d13efe4ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852997"
---
# <a name="bitsadmin-sethelpertoken"></a>Bitsadmin sethelpertoken

Define o token de principal do prompt de comando atual (ou token da conta de usuário local arbitrário, se especificado) como um trabalho de transferência de BITS [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs).

**BITS 3.0 e versões anteriores**: Sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho.|
|\<username@domain\> \<password\>|Opcional&mdash;as credenciais de um usuário local conta cujo token a ser usado.|

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
