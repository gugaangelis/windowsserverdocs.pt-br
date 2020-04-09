---
title: bitsadmin sethelpertoken
description: O tópico de comandos do Windows para Bitsadmin sethelpertoken, que define o token primário do prompt de comando atual (ou um token de conta de usuário local arbitrário, se especificado) como um token auxiliar do trabalho de transferência de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a1e8fd0054cadf3bf06b6e5b7bdf5010b18781e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849529"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

Define o token primário do prompt de comando atual (ou um token de conta de usuário local arbitrário, se especificado) como um [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)do trabalho de transferência de bits.

**BITS 3,0 e anteriores**: sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho.|
|\<username@domain\> \<senha\>|Opcional&mdash;as credenciais de uma conta de usuário local cujo token deve ser usado.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
