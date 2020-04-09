---
title: bitsadmin sethelpertokenflags
description: O tópico de comandos do Windows para Bitsadmin sethelpertokenflags, que define os sinalizadores de uso para um token auxiliar que está associado a um trabalho de transferência de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: c644e82026cfc1d62f3fb5d20e3925002b871036
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849489"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

Define os sinalizadores de uso para um [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs) que está associado a um trabalho de transferência bits.

**BITS 3,0 e anteriores**: sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho.|
|Sinalizadores|Os valores possíveis incluem o seguinte. 0x0001&mdash;o token auxiliar é usado para abrir o arquivo local de um trabalho de upload, para criar ou renomear o arquivo temporário de um trabalho de download ou para criar ou renomear o arquivo de resposta de um trabalho de resposta de upload. 0x0002&mdash;o token auxiliar é usado para abrir o arquivo remoto de um carregamento do protocolo SMB ou um trabalho de download, ou em resposta a um servidor HTTP ou a um desafio de proxy para credenciais NTLM ou Kerberos implícitas. Você deve chamar `/SetCredentialsJob TargetScheme NULL NULL` para permitir que as credenciais sejam enviadas via HTTP.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
