---
title: nslookup root
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3eb3375df3a109685fc8dc5d23f0c5008339d09e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373393"
---
# <a name="nslookup-root"></a>nslookup root

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o servidor padrão para o servidor para a raiz do espaço de nome de domínio DNS (sistema de nomes de domínio).
## <a name="syntax"></a>Sintaxe
```
root 
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                      Descrição                      |
|-----------------|-------------------------------------------------------|
| {ajuda &#124; ?} | Exibe um breve resumo dos subcomandos **nslookup** . |

## <a name="remarks"></a>Comentários
- Atualmente, o servidor de nome ns.nic.ddn.mil é usado. Esse comando é um sinônimo para o lserver ns.nic.ddn.mil. Você pode alterar o nome do servidor raiz com o comando **set root** .
  ## <a name="additional-references"></a>referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set root](nslookup-set-root.md)
