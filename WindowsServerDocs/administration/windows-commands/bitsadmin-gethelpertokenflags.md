---
title: bitsadmin gethelpertokenflags
description: O tópico de comandos do Windows para **Bitsadmin gethelpertokenflags** -retorna os sinalizadores de uso para um token auxiliar que está associado a um trabalho de transferência de bits.
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
ms.openlocfilehash: 25d667736d5fdcd018f557b2a5565b94898f6e51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381572"
---
# <a name="bitsadmin-gethelpertokenflags"></a>bitsadmin gethelpertokenflags

Retorna os sinalizadores de uso para um [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)@no__t-qual está associado a um trabalho de transferência bits.

**BITS 3,0 e anteriores**: Não compatível.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetHelperTokenFlags <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Os valores de retorno possíveis incluem o seguinte.

- 0x0001. O token auxiliar é usado para abrir o arquivo local de um trabalho de upload, para criar ou renomear o arquivo temporário de um trabalho de download ou para criar ou renomear o arquivo de resposta de um trabalho de resposta de upload.
- 0x0002. O token auxiliar é usado para abrir o arquivo remoto de um trabalho de upload ou download do protocolo SMB ou em resposta a um servidor HTTP ou a um desafio de proxy para credenciais NTLM ou Kerberos implícitas. Você deve chamar/SetCredentialsJob TargetScheme NULL NULL para permitir que as credenciais sejam enviadas via HTTP.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
