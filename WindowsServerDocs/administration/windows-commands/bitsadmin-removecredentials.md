---
title: bitsadmin removecredentials
description: Tópico de comandos do Windows para Bitsadmin **removecredentials**, que remove as credenciais de um trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55ff7e2a813c7cc6b60e04d55ef63804a2aed796
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849839"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Remove as credenciais de um trabalho.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /removecredentials <job> <target> <scheme>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| target | Use o **servidor** ou **proxy**. |
| scheme | Use um dos seguintes:<ul><li>**Basic.** Esquema de autenticação em que o nome de usuário e a senha são enviados em texto não criptografado para o servidor ou proxy.</li><li>**Digest.** Um esquema de autenticação de desafio/resposta que usa uma cadeia de caracteres de dados especificada pelo servidor para o desafio.</li><li>**NTLM.** Um esquema de autenticação de desafio/resposta que usa as credenciais do usuário para autenticação em um ambiente de rede do Windows.</li><li>**Negotiate (também conhecido como protocolo de negociação simples e protegido).** Um esquema de autenticação de desafio/resposta que negocia com o servidor ou proxy para determinar qual esquema usar para autenticação. Os exemplos são o protocolo Kerberos e NTLM.</li><li>**passaporte.** Um serviço de autenticação centralizado fornecido pela Microsoft que oferece um logon único para sites membros.</li></ul> |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir remove as credenciais do trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /removecredentials myDownloadJob server basic
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)