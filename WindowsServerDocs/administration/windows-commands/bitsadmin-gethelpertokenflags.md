---
title: Bitsadmin gethelpertokenflags
description: Tópico de comandos do Windows para **gethelpertokenflags bitsadmin** -retorna os sinalizadores de uso para um token auxiliar que está associado um trabalho de transferência do BITS.
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
ms.openlocfilehash: e5665ed4670891dcbecd56215342f3d94e1ed753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885787"
---
# <a name="bitsadmin-gethelpertokenflags"></a>Bitsadmin gethelpertokenflags

Retorna os sinalizadores de uso para um [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) associado a um trabalho de transferência do BITS.

**BITS 3.0 e versões anteriores**: Sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetHelperTokenFlags <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Os valores de retornados possíveis incluem o seguinte.

- 0x0001. O token auxiliar é usado para abrir o arquivo local de um trabalho de upload, criar ou renomear o arquivo temporário de um trabalho de download, ou para criar ou renomear o arquivo de resposta de um trabalho de resposta de upload.
- 0x0002. O token auxiliar é usado para abrir o arquivo remoto de um carregamento de bloco de mensagens de servidor (SMB) ou baixar o trabalho ou em resposta a um desafio de servidor ou proxy HTTP para implícita NTLM ou Kerberos de credenciais. Você deve chamar SetCredentialsJob TargetScheme nulo nulo para permitir que as credenciais a serem enviados via HTTP.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
