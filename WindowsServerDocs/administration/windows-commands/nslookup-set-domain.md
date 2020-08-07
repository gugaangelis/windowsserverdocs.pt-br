---
title: nslookup set domain
description: Artigo de referência para o comando Set Domain do nslookup, que altera o nome de domínio padrão do DNS (sistema de nomes de domínio) para o nome especificado.
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fbf2602f387af9a1f389bdcccc50b19a5b25c2ce
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885667"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o nome de domínio padrão do DNS (sistema de nomes de domínio) para o nome especificado.

## <a name="syntax"></a>Sintaxe

```
set domain=<domainname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<domainname>` | Especifica um novo nome para o nome de domínio DNS padrão. O valor padrão é o nome do host. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O nome de domínio DNS padrão é anexado a uma solicitação de pesquisa, dependendo do estado das **defname** e das opções de **pesquisa** .

- A lista de pesquisa de domínio DNS contém os pais do domínio DNS padrão se ele tiver pelo menos dois componentes em seu nome. Por exemplo, se o domínio DNS padrão for mfg.widgets.com, a lista de pesquisa será nomeada mfg.widgets.com e widgets.com.

- Use o comando [set srchlist do nslookup](nslookup-set-srchlist.md) para especificar uma lista diferente e o comando [set All do nslookup](nslookup-set-all.md) para exibir a lista.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup set srchlist](nslookup-set-srchlist.md)

- [nslookup set all](nslookup-set-all.md)
