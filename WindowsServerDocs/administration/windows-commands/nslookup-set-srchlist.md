---
title: nslookup set srchlist
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb93a9f7cf969161536e88bec929b7e6ba0f0e5d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372774"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o nome de domínio padrão do DNS (sistema de nomes de domínio) e a lista de pesquisa.

## <a name="syntax"></a>Sintaxe
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                                                                        Descrição                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica novos nomes para o domínio DNS padrão e a lista de pesquisa. O valor do nome de domínio padrão é baseado no nome do host. Você pode especificar um máximo de seis nomes separados por barras (/). |
| {ajuda &#124; ?} |                                                                   Exibe um breve resumo dos subcomandos **nslookup** .                                                                   |

## <a name="remarks"></a>Comentários
- O comando **set srchlist**substitui o nome de domínio DNS padrão e a lista de pesquisa do comando **set Domain** . Use o comando **set All** para exibir a lista.
  ## <a name="BKMK_examples"></a>Disso
  O exemplo a seguir define o domínio DNS como mfg.widgets.com e a lista de pesquisa com os três nomes:
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set Domain](nslookup-set-domain.md)
  [nslookup Set All](nslookup-set-all.md)
