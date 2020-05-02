---
title: nslookup server
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9a3f4c7bdbcf8122bb532fafe83b400400d8d6b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723680"
---
# <a name="nslookup-server"></a>nslookup server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o domínio DNS (sistema de nomes de domínio) especificado.
## <a name="syntax"></a>Sintaxe
```
server <DNSDomain>
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                          Descrição                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obrigatórios. Especifica o novo domínio DNS para o servidor padrão. |
| {ajuda &#124;?} |     Exibe um breve resumo dos subcomandos **nslookup** .      |

## <a name="remarks"></a>Comentários
- O comando de **servidor** usa o servidor padrão atual para pesquisar as informações sobre o domínio DNS especificado. Isso é diferente do comando **lserver** , que usa o servidor inicial.
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
