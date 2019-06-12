---
title: nslookup lserver
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2f2f787915f2b941d6c098d44de1bb0e04dbd491
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436915"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o domínio especificado do sistema de nome de domínio (DNS).
## <a name="syntax"></a>Sintaxe
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                      Descrição                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Especifica o novo domínio DNS para o servidor padrão.  |
| {Ajuda &#124; ?} | Exibe um resumo breve dos **nslookup** subcomandos. |

## <a name="remarks"></a>Comentários
- O **lserver** comando usa o servidor inicial para pesquisar as informações sobre o domínio DNS especificado. Isso está em contraste com o **server** comando, que usa o servidor padrão atual.
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [server nslookup](nslookup-server.md)
