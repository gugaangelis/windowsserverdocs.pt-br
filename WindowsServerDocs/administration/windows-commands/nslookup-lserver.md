---
title: nslookup lserver
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 347ad6e380f8d8163c4954771c9e985271b2d549
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373083"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o domínio DNS (sistema de nomes de domínio) especificado.
## <a name="syntax"></a>Sintaxe
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                      Descrição                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Especifica o novo domínio DNS para o servidor padrão.  |
| {ajuda &#124; ?} | Exibe um breve resumo dos subcomandos **nslookup** . |

## <a name="remarks"></a>Comentários
- O comando **lserver** usa o servidor inicial para pesquisar as informações sobre o domínio DNS especificado. Isso é diferente do comando de **servidor** , que usa o servidor padrão atual.
  ## <a name="additional-references"></a>referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [servidor nslookup](nslookup-server.md)
