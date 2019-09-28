---
title: bitsadmin removecredentials
description: Tópico de comandos do Windows para **Bitsadmin removecredentials** – remove as credenciais de um trabalho.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34a5bae9304a9db9f47f437276270ca06b1ebeee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380818"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Remove as credenciais de um trabalho.

**BITS 1,2 e anteriores**: Não compatível.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /RemoveCredentials <Job> <Target> <Scheme>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Destino|SERVIDOR ou PROXY|
|esquema|Uma das seguintes opções:</br>-BÁSICO – esquema de autenticação no qual o nome de usuário e a senha são enviados em texto não criptografado para o servidor ou proxy.</br>-DIGEST — um esquema de autenticação de desafio/resposta que usa uma cadeia de caracteres de dados especificada pelo servidor para o desafio.</br>-NTLM — um esquema de autenticação de desafio/resposta que usa as credenciais do usuário para autenticação em um ambiente de rede do Windows.</br>-NEGOTIAte — também conhecido como Snego (protocolo de negociação simples e protegido) é um esquema de autenticação de desafio/resposta que negocia com o servidor ou proxy para determinar qual esquema usar para autenticação. Os exemplos são o protocolo Kerberos e NTLM.</br>-PASSPORT — um serviço de autenticação centralizado fornecido pela Microsoft que oferece um logon único para sites membros.|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir remove as credenciais do trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)