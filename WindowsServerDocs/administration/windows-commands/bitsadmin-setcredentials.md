---
title: bitsadmin setcredentials
description: Tópico de comandos do Windows para Bitsadmin SetCredentials, que adiciona credenciais a um trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 918bda93407e029cedaaf5eab937d1bb23dc3c4c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849639"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Adiciona credenciais a um trabalho.

**BITS 1,2 e anteriores**: sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetCredentials <Job> <Target> <Scheme> <Username> <Password>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Destino|SERVIDOR ou PROXY|
|Esquema|Um das seguintes condições:</br>-BÁSICO – esquema de autenticação no qual o nome de usuário e a senha são enviados em texto não criptografado para o servidor ou proxy.</br>-DIGEST — um esquema de autenticação de desafio/resposta que usa uma cadeia de caracteres de dados especificada pelo servidor para o desafio.</br>-NTLM — um esquema de autenticação de desafio/resposta que usa as credenciais do usuário para autenticação em um ambiente de rede do Windows.</br>-NEGOTIAte — também conhecido como Snego (protocolo de negociação simples e protegido) é um esquema de autenticação de desafio/resposta que negocia com o servidor ou proxy para determinar qual esquema usar para autenticação. Os exemplos são o protocolo Kerberos e NTLM.</br>-PASSPORT — um serviço de autenticação centralizado fornecido pela Microsoft que oferece um logon único para sites membros.|
|Nome de Usuário|O nome das credenciais fornecidas|
|Senha|A senha associada ao nome de *usuário* fornecido|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir adiciona as credenciais ao trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /RemoveCredentials myDownloadJob SERVER BASIC Edward Password20
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)