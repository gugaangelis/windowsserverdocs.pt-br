---
title: nslookup server
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8de2d8a801d57a9197d5e8ec7d3b6adb2d00dd04
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838679"
---
# <a name="nslookup-server"></a>nslookup server

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o domínio DNS (sistema de nomes de domínio) especificado.
## <a name="syntax"></a>Sintaxe
```
server <DNSDomain>
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                          Descrição                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obrigatório. Especifica o novo domínio DNS para o servidor padrão. |
| {ajuda &#124; ?} |     Exibe um breve resumo dos subcomandos **nslookup** .      |

## <a name="remarks"></a>Comentários
- O comando de **servidor** usa o servidor padrão atual para pesquisar as informações sobre o domínio DNS especificado. Isso é diferente do comando **lserver** , que usa o servidor inicial.
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
