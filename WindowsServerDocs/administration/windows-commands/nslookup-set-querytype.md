---
title: nslookup set querytype
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc992d83de8537c285b6d2d97e5f44545e2f930f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723604"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o tipo de registro de recurso para a consulta.
## <a name="syntax"></a>Sintaxe
```
set querytype=<ResourceRecordtype>
```
### <a name="parameters"></a>Parâmetros
<ResourceRecordtype>Especifica um tipo de registro de recurso DNS. O tipo de registro de recurso padrão é um. A tabela a seguir lista os valores válidos para esse comando.

| Valor |                                                   Descrição                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   Um   |                                      Especifica um endereço IP&#39;s de computador                                      |
|  ANY  |                                     Especifica um endereço IP&#39;s de computador.                                      |
| CNAME |                                    Especifica um nome canônico para um alias.                                     |
|  GID  |                                  Especifica um identificador de grupo de um nome de grupo.                                  |
| HINFO |                          Especifica um computador&#39;s CPU e o tipo de sistema operacional.                           |
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
  ## <a name="additional-references"></a>Referências adicionais
  <a href = Command-Line-Syntax-key.md data-RAW-Source =- [Command-Line Syntax key](command-line-syntax-key.md)>chave</a> de sintaxe de linha de comando <a href = nslookup-Set-Type.MD data-RAW-Source =[nslookup set type](nslookup-set-type.md)>nslookup set type</a>
