---
title: nslookup server
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ce609f62f2d87024e1d75b43869b3b867e946e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373061"
---
# <a name="nslookup-server"></a>nslookup server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o domínio DNS (sistema de nomes de domínio) especificado.
## <a name="syntax"></a>Sintaxe
```
server <DNSDomain>
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                          Descrição                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obrigatório. Especifica o novo domínio DNS para o servidor padrão. |
| {ajuda &#124; ?} |     Exibe um breve resumo dos subcomandos **nslookup** .      |

## <a name="remarks"></a>Comentários
- O comando de **servidor** usa o servidor padrão atual para pesquisar as informações sobre o domínio DNS especificado. Isso é diferente do comando **lserver** , que usa o servidor inicial.
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)[nslookup lserver](nslookup-lserver.md) 
  
