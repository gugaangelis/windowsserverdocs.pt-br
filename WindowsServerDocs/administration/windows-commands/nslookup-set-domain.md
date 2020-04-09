---
title: nslookup set domain
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa433383e23fd19779960348e0af88e0a405ff83
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838459"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o nome de domínio padrão do DNS (sistema de nomes de domínio) para o nome especificado.
## <a name="syntax"></a>Sintaxe
```
set domain=<DomainName>
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                           Descrição                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica um novo nome para o nome de domínio DNS padrão. O nome de domínio padrão é o nome do host. |
| {ajuda &#124; ?} |                      Exibe um breve resumo dos subcomandos **nslookup** .                      |

## <a name="remarks"></a>Comentários
- O nome de domínio DNS padrão é anexado a uma solicitação de pesquisa, dependendo do estado das **defname** e das opções de **pesquisa** . A lista de pesquisa de domínio DNS contém os pais do domínio DNS padrão se ele tiver pelo menos dois componentes em seu nome. Por exemplo, se o domínio DNS padrão for mfg.widgets.com, a lista de pesquisa será nomeada mfg.widgets.com e widgets.com. Use o comando **set srchlist** para especificar uma lista diferente e o comando **set All** para exibir a lista.
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set srchlist](nslookup-set-srchlist.md)
  [nslookup Set All](nslookup-set-all.md)
