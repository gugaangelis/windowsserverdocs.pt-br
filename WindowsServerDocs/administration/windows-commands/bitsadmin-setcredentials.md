---
title: bitsadmin setcredentials
description: Tópico de comandos do Windows para **bitsadmin setcredentials** -adiciona credenciais a um trabalho.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 923dcff7d268d40b72db3254e2a97c808c7c7253
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877387"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Adiciona as credenciais a um trabalho.

**BITS 1.2 e anteriores**: Sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Destino|SERVIDOR ou PROXY|
|Esquema|Uma das seguintes opções:</br>-BÁSICO — o esquema de autenticação no qual o nome de usuário e senha são enviados em texto não criptografado para o servidor ou proxy.</br>-DIGEST — um esquema de autenticação de desafio / resposta que usa uma cadeia de caracteres de dados do servidor especificado para o desafio.</br>-NTLM — um esquema de autenticação de desafio / resposta que usa as credenciais do usuário para autenticação em um ambiente de rede do Windows.</br>-NEGOTIATE — também conhecido como o protocolo simples e protegido negociação (Snego) é um esquema de autenticação de desafio / resposta que negocia com o servidor ou proxy para determinar o esquema a ser usado para autenticação. Os exemplos são o protocolo Kerberos e NTLM.</br>-O PASSPORT — um serviço de autenticação centralizado fornecido pela Microsoft que oferece um único logon para sites de membros.|
|Nome de usuário|O nome das credenciais fornecidas|
|Senha|A senha associada fornecido *nome de usuário*|

## <a name="BKMK_examples"></a>Exemplos

As seguintes credenciais de Adds de exemplo para trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)