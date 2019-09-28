---
title: bitsadmin sethelpertokenflags
description: O tópico de comandos do Windows para **Bitsadmin sethelpertokenflags** – define os sinalizadores de uso para um token auxiliar que está associado a um trabalho de transferência de bits.
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
ms.openlocfilehash: 6047c63677fac3311634ababb675be5301b7f3b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380576"
---
# <a name="bitsadmin-sethelpertokenflags"></a>bitsadmin sethelpertokenflags

Define os sinalizadores de uso para um [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)@no__t-qual está associado a um trabalho de transferência bits.

**BITS 3,0 e anteriores**: Não compatível.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetHelperTokenFlags <Job> <Flags>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho.|
|Sinalizadores|Os valores possíveis incluem o seguinte. o token auxiliar 0x0001 @ no__t-0The é usado para abrir o arquivo local de um trabalho de upload, para criar ou renomear o arquivo temporário de um trabalho de download ou para criar ou renomear o arquivo de resposta de um trabalho de resposta de upload. o token auxiliar 0x0002 @ no__t-0The é usado para abrir o arquivo remoto de um carregamento do protocolo SMB ou um trabalho de download, ou em resposta a um servidor HTTP ou a um desafio de proxy para credenciais NTLM ou Kerberos implícitas. Você deve chamar @ no__t-0 @ no__t-1to permitir que as credenciais sejam enviadas via HTTP.|

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
