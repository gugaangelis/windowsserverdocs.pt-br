---
title: nslookup lserver
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2054c0fd427b41e7d6076258b29ab78d0fb7892
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723671"
---
# <a name="nslookup-lserver"></a>nslookup lserver

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o domínio DNS (sistema de nomes de domínio) especificado.
## <a name="syntax"></a>Sintaxe
```
lserver <DNSDomain> 
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                      Descrição                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Especifica o novo domínio DNS para o servidor padrão.  |
| {ajuda &#124;?} | Exibe um breve resumo dos subcomandos **nslookup** . |

## <a name="remarks"></a>Comentários
- O comando **lserver** usa o servidor inicial para pesquisar as informações sobre o domínio DNS especificado. Isso é diferente do comando de **servidor** , que usa o servidor padrão atual.
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [servidor nslookup](nslookup-server.md)
