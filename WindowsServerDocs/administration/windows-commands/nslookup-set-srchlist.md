---
title: nslookup set srchlist
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39b28e7d43df2427caae46d323cd30f03b6b484c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436577"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera a lista de pesquisa e de nome de domínio do sistema de nome de domínio (DNS) padrão.

## <a name="syntax"></a>Sintaxe
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                                                                        Descrição                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica novos nomes para a lista de domínio e a pesquisa DNS padrão. O valor de nome de domínio padrão baseia-se no nome do host. Você pode especificar um máximo de seis nomes separados por barras (/). |
| {Ajuda &#124; ?} |                                                                   Exibe um resumo breve dos **nslookup** subcomandos.                                                                   |

## <a name="remarks"></a>Comentários
- O **definir srchlist**comando substitui o domínio DNS padrão nome e a pesquisa de lista da **domínio conjunto** comando. Use o **definir tudo** comando para exibir a lista.
  ## <a name="BKMK_examples"></a>Exemplos
  O exemplo a seguir define o domínio DNS para mfg.widgets.com e a lista de pesquisa para os três nomes:
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup definir domínio](nslookup-set-domain.md)
  [nslookup definir todos](nslookup-set-all.md)
