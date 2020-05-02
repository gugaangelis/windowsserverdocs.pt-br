---
title: nslookup set domain
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6ee52cd5ad35dcfc6da1cd3885f66124338d62f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723613"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o nome de domínio padrão do DNS (sistema de nomes de domínio) para o nome especificado.
## <a name="syntax"></a>Sintaxe
```
set domain=<DomainName>
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                           Descrição                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica um novo nome para o nome de domínio DNS padrão. O nome de domínio padrão é o nome do host. |
| {ajuda &#124;?} |                      Exibe um breve resumo dos subcomandos **nslookup** .                      |

## <a name="remarks"></a>Comentários
- O nome de domínio DNS padrão é anexado a uma solicitação de pesquisa, dependendo do estado das **defname** e das opções de **pesquisa** . A lista de pesquisa de domínio DNS contém os pais do domínio DNS padrão se ele tiver pelo menos dois componentes em seu nome. Por exemplo, se o domínio DNS padrão for mfg.widgets.com, a lista de pesquisa será nomeada mfg.widgets.com e widgets.com. Use o comando **set srchlist** para especificar uma lista diferente e o comando **set All** para exibir a lista.
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set srchlist](nslookup-set-srchlist.md)
  [nslookup Set All](nslookup-set-all.md)
