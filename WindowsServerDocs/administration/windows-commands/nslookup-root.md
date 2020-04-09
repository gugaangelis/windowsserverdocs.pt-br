---
title: nslookup root
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d11179ff3cd22acd9df67261e7ab752aa159201a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838639"
---
# <a name="nslookup-root"></a>nslookup root

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o servidor padrão para o servidor para a raiz do espaço de nome de domínio DNS (sistema de nomes de domínio).
## <a name="syntax"></a>Sintaxe
```
root 
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                      Descrição                      |
|-----------------|-------------------------------------------------------|
| {ajuda &#124; ?} | Exibe um breve resumo dos subcomandos **nslookup** . |

## <a name="remarks"></a>Comentários
- Atualmente, o servidor de nome ns.nic.ddn.mil é usado. Esse comando é um sinônimo para o lserver ns.nic.ddn.mil. Você pode alterar o nome do servidor raiz com o comando **set root** .
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set root](nslookup-set-root.md)
