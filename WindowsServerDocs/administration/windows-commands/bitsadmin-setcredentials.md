---
title: bitsadmin setcredentials
description: O tópico de comandos do Windows para **Bitsadmin SetCredentials** – adiciona credenciais a um trabalho.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70ac9a01a2e713b5a2fb881f327a52552a6bbec6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380717"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Adiciona credenciais a um trabalho.

**BITS 1,2 e anteriores**: Não compatível.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Destino|SERVIDOR ou PROXY|
|esquema|Uma das seguintes opções:</br>-BÁSICO – esquema de autenticação no qual o nome de usuário e a senha são enviados em texto não criptografado para o servidor ou proxy.</br>-DIGEST — um esquema de autenticação de desafio/resposta que usa uma cadeia de caracteres de dados especificada pelo servidor para o desafio.</br>-NTLM — um esquema de autenticação de desafio/resposta que usa as credenciais do usuário para autenticação em um ambiente de rede do Windows.</br>-NEGOTIAte — também conhecido como Snego (protocolo de negociação simples e protegido) é um esquema de autenticação de desafio/resposta que negocia com o servidor ou proxy para determinar qual esquema usar para autenticação. Os exemplos são o protocolo Kerberos e NTLM.</br>-PASSPORT — um serviço de autenticação centralizado fornecido pela Microsoft que oferece um logon único para sites membros.|
|Nome de usuário|O nome das credenciais fornecidas|
|Senha|A senha associada ao nome de *usuário* fornecido|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir adiciona as credenciais ao trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)