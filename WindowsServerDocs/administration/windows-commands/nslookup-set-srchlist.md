---
title: nslookup set srchlist
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8936daa3505b02295ae2f09c2910dead8d4c0ff8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723549"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o nome de domínio padrão do DNS (sistema de nomes de domínio) e a lista de pesquisa.

## <a name="syntax"></a>Sintaxe
```
Set srchlist=<DomainName>[/...]
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                                                                        Descrição                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica novos nomes para o domínio DNS padrão e a lista de pesquisa. O valor do nome de domínio padrão é baseado no nome do host. Você pode especificar um máximo de seis nomes separados por barras (/). |
| {ajuda &#124;?} |                                                                   Exibe um breve resumo dos subcomandos **nslookup** .                                                                   |

## <a name="remarks"></a>Comentários
- O comando **set srchlist**substitui o nome de domínio DNS padrão e a lista de pesquisa do comando **set Domain** . Use o comando **set All** para exibir a lista.
  ## <a name="examples"></a>Exemplos
  Para definir o domínio DNS como mfg.widgets.com e a lista de pesquisa com os três nomes:
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set Domain](nslookup-set-domain.md)
  [nslookup Set All](nslookup-set-all.md)
