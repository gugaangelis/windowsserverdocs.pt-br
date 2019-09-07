---
title: nslookup set querytype
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 496eededd8b0b5eb79cdc1b4a7e35bc017157768
ms.sourcegitcommit: f3b61dcd8aa0aa744db4ea938aac633c19217b0a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746307"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o tipo de registro de recurso para a consulta.
## <a name="syntax"></a>Sintaxe
```
set querytype=<ResourceRecordtype>
```
## <a name="parameters"></a>Parâmetros
<ResourceRecordtype>Especifica um tipo de registro de recurso DNS. O tipo de registro de recurso padrão é um. A tabela a seguir lista os valores válidos para esse comando.

| Valor |                                                   Descrição                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      Especifica o endereço&#39;IP de um computador                                      |
|  OUTRO  |                                     Especifica o endereço&#39;IP de um computador.                                      |
| CNAME |                                    Especifica um nome canônico para um alias.                                     |
|  GID  |                                  Especifica um identificador de grupo de um nome de grupo.                                  |
| HINFO |                          Especifica a CPU&#39;e o tipo de sistema operacional do computador.                           |
|  MB   |                                        Especifica um nome de domínio de caixa de correio.                                         |
|  MG   |                                         Especifica um membro do grupo de email.                                          |
| MINFO |                                   Especifica informações da caixa de correio ou da lista de mensagens.                                   |
|  MR   |                                     Especifica o nome de domínio de renomeação de email.                                      |
|  MX   |                                          Especifica o trocador de mensagens.                                          |
|  NS   |                                 Especifica um servidor de nomes DNS para a zona nomeada.                                 |
|  PTR  | Especifica um nome de computador se a consulta for um endereço IP; caso contrário, especifica o ponteiro para outras informações. |
|  SOA  |                                Especifica o início de autoridade para uma zona DNS.                                 |
|  TXT  |                                         Especifica as informações de texto.                                         |
|  UID  |                                         Especifica o identificador de usuário.                                          |
| UINFO |                                         Especifica as informações do usuário.                                         |
|  WKS  |                                         Descreve um serviço conhecido.                                         |
| {ajuda |                                                       ?}                                                        |

Exibe um breve resumo dos subcomandos <strong>nslookup</strong>
## <a name="remarks"></a>Comentários
- O comando <strong>set type</strong> executa a mesma função que o comando <strong>set QueryType</strong> .
- Para obter mais informações sobre tipos de registro de recurso, consulte solicitação de comentário (RFC) 1035.
  ## <a name="additional-references"></a>referências adicionais
  <a href="command-line-syntax-key.md" data-raw-source="[Command-Line Syntax Key](command-line-syntax-key.md)">Chave de sintaxe de linha de comando</a><a href="nslookup-set-type.md" data-raw-source="[nslookup set type](nslookup-set-type.md)">nslookup set type</a> 
  
