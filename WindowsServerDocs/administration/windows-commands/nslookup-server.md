---
title: nslookup server
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ba58e223d0aa35b4157b813b10bf1d274313a1c1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436974"
---
# <a name="nslookup-server"></a>nslookup server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o domínio especificado do sistema de nome de domínio (DNS).
## <a name="syntax"></a>Sintaxe
```
server <DNSDomain>
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                          Descrição                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obrigatório. Especifica o novo domínio DNS para o servidor padrão. |
| {Ajuda &#124; ?} |     Exibe um resumo breve dos **nslookup** subcomandos.      |

## <a name="remarks"></a>Comentários
- O **server** comando usa o servidor padrão atual para pesquisar as informações sobre o domínio DNS especificado. Isso está em contraste com o **lserver** comando, que usa o servidor inicial.
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [lserver nslookup](nslookup-lserver.md)
