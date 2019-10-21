---
title: nslookup set root
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a1737275bf6321525bbba56cd4d6a77ef973423
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591022"
---
# <a name="nslookup-set-root"></a>nslookup set root

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o nome do servidor raiz usado para consultas.
## <a name="syntax"></a>Sintaxe
```
set root=<RootServer>
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                   Descrição                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Especifica o novo nome para o servidor raiz. O valor padrão é ns.nic.ddn.mil. |
| {ajuda &#124; ?} |              Exibe um breve resumo dos subcomandos **nslookup** .               |

## <a name="remarks"></a>Comentários
- O subcomando **set root** afeta o subcomando **raiz** .
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md) 
  [raiz do nslookup](nslookup-root.md)
