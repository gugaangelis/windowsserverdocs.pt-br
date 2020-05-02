---
title: nslookup root
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bdfbe40443cf8f2fec2f81608bb93603cd74937f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723697"
---
# <a name="nslookup-root"></a>nslookup root

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o servidor padrão para o servidor para a raiz do espaço de nome de domínio DNS (sistema de nomes de domínio).
## <a name="syntax"></a>Sintaxe
```
root 
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                      Descrição                      |
|-----------------|-------------------------------------------------------|
| {ajuda &#124;?} | Exibe um breve resumo dos subcomandos **nslookup** . |

## <a name="remarks"></a>Comentários
- Atualmente, o servidor de nome ns.nic.ddn.mil é usado. Esse comando é um sinônimo para o lserver ns.nic.ddn.mil. Você pode alterar o nome do servidor raiz com o comando **set root** .
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set root](nslookup-set-root.md)
