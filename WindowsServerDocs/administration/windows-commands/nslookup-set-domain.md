---
title: nslookup set domain
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1af9f30dd2c44111adecb477a6469333f4f7685
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436772"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o nome de domínio do sistema de nome de domínio (DNS) padrão para o nome especificado.
## <a name="syntax"></a>Sintaxe
```
set domain=<DomainName>
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                           Descrição                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica um novo nome para o nome de domínio DNS padrão. O nome de domínio padrão é o nome do host. |
| {Ajuda &#124; ?} |                      Exibe um resumo breve dos **nslookup** subcomandos.                      |

## <a name="remarks"></a>Comentários
- O nome de domínio DNS padrão é acrescentado a uma solicitação de pesquisa dependendo do estado do **defname** e **pesquisa** opções. A lista de pesquisa de domínio DNS contém o pai do domínio DNS padrão se ele tiver pelo menos dois componentes em seu nome. Por exemplo, se o domínio DNS padrão for mfg.widgets.com, a lista de pesquisa é denominada mfg.widgets.com e widgets.com. Use o **definir srchlist** comando para especificar uma lista diferente e o **definir tudo** comando para exibir a lista.
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup definir srchlist](nslookup-set-srchlist.md)
  [nslookup definir todos](nslookup-set-all.md)
