---
title: nslookup lserver
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d0d8619101d2e7b1f7fb6d6ed99d801c7c264f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838629"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o domínio DNS (sistema de nomes de domínio) especificado.
## <a name="syntax"></a>Sintaxe
```
lserver <DNSDomain> 
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                      Descrição                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Especifica o novo domínio DNS para o servidor padrão.  |
| {ajuda &#124; ?} | Exibe um breve resumo dos subcomandos **nslookup** . |

## <a name="remarks"></a>Comentários
- O comando **lserver** usa o servidor inicial para pesquisar as informações sobre o domínio DNS especificado. Isso é diferente do comando de **servidor** , que usa o servidor padrão atual.
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [servidor nslookup](nslookup-server.md)
