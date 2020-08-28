---
title: nslookup set srchlist
description: Artigo de referência para o comando Set srchlist do nslookup, que altera o nome de domínio padrão do DNS (sistema de nomes de domínio) e a lista de pesquisa.
ms.topic: reference
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45f829337cca347b1503aca0fb02a1c77359914a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037284"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o nome de domínio padrão do DNS (sistema de nomes de domínio) e a lista de pesquisa. Esse comando substitui o nome de domínio DNS padrão e a lista de pesquisa do comando [set Domain do nslookup](nslookup-set-domain.md) .

## <a name="syntax"></a>Sintaxe

```
set srchlist=<domainname>[/...]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<domainname>` | Especifica novos nomes para o domínio DNS padrão e a lista de pesquisa. O valor do nome de domínio padrão é baseado no nome do host. Você pode especificar um máximo de seis nomes separados por barras (/). |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Use o comando [set All do nslookup](nslookup-set-all.md) para exibir a lista.

### <a name="examples"></a>Exemplos

Para definir o domínio DNS como *MFG.widgets.com* e a lista de pesquisa com os três nomes:

```
set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup set domain](nslookup-set-domain.md)

- [nslookup set all](nslookup-set-all.md)
