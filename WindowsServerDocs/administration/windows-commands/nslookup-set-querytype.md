---
title: nslookup set querytype
description: Artigo de referência para o comando Set QueryType do nslookup, que altera o tipo de registro de recurso para a consulta.
ms.topic: reference
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b75fd9f80de828b6927289480351db106ba50fd6
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038737"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o tipo de registro de recurso para a consulta. Para obter informações sobre tipos de registro de recurso, consulte [solicitação de comentário (RFC) 1035](https://tools.ietf.org/html/rfc1035).

> [!NOTE]
> Esse comando é o mesmo que o comando de [tipo set do nslookup](nslookup-set-type.md) .

## <a name="syntax"></a>Sintaxe

```
set querytype=<resourcerecordtype>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<resourcerecordtype>` | Especifica um tipo de registro de recurso DNS. O tipo de registro de recurso padrão é **um**, mas você pode usar qualquer um dos seguintes valores:<ul><li>**R:** Especifica o endereço IP de um computador.</li><li>**Qualquer:** Especifica o endereço IP de um computador.</li><li>**CNAME:** Especifica um nome canônico para um alias.</li><li>**GID** Especifica um identificador de grupo de um nome de grupo.</li><li>**HINFO:** Especifica a CPU e o tipo de sistema operacional de um computador.</li><li>**MB:** Especifica um nome de domínio de caixa de correio.</li><li>**Mg:** Especifica um membro do grupo de email.</li><li>**MINFO:** Especifica informações da caixa de correio ou da lista de mensagens.</li><li>**Mr:** Especifica o nome de domínio de renomeação de email.</li><li>**MX:** Especifica o trocador de mensagens.</li><li>**Ns:** Especifica um servidor de nomes DNS para a zona nomeada.</li><li>**PTR:** Especifica um nome de computador se a consulta for um endereço IP; caso contrário, especifica o ponteiro para outras informações.</li><li>**Soa:** Especifica o início de autoridade para uma zona DNS.</li><li>**Txt:** Especifica as informações de texto.</li><li>**UID:** Especifica o identificador de usuário.</li><li>**UINFO:** Especifica as informações do usuário.</li><li>**WKS:** Descreve um serviço conhecido.</li></ul> |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup set type](nslookup-set-type.md)
