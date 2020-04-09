---
title: bitsadmin gethelpertokenflags
description: O tópico de comandos do Windows para **Bitsadmin gethelpertokenflags**, que retorna os sinalizadores de uso para um token auxiliar que está associado a um trabalho de transferência de bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 37e40ea1f71795e0c975988169577deb205c3335
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850659"
---
# <a name="bitsadmin-gethelpertokenflags"></a>bitsadmin gethelpertokenflags

Retorna os sinalizadores de uso para um [token auxiliar](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs) associado a um trabalho de transferência bits.

> [!NOTE]
> Esse comando não tem suporte no BITS 3,0 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gethelpertokenflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="remarks"></a>Comentários

Possíveis valores de retorno, incluindo:

- **0x0001.** O token auxiliar é usado para abrir o arquivo local de um trabalho de upload, para criar ou renomear o arquivo temporário de um trabalho de download ou para criar ou renomear o arquivo de resposta de um trabalho de resposta de upload.

- **0x0002.** O token auxiliar é usado para abrir o arquivo remoto de um trabalho de upload ou download do protocolo SMB ou em resposta a um servidor HTTP ou a um desafio de proxy para credenciais NTLM ou Kerberos implícitas. Você deve chamar/SetCredentialsJob TargetScheme NULL NULL para permitir que as credenciais sejam enviadas via HTTP.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
