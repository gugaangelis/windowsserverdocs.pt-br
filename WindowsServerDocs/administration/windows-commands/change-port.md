---
title: change port
description: Tópico de comandos do Windows para porta de alteração, que lista ou altera os mapeamentos de porta COM para que sejam compatíveis com os aplicativos do MS-DOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39273382038edb7709f2d99baea8090d71df3a57
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848069"
---
# <a name="change-port"></a>change port

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lista ou altera os mapeamentos de porta COM para que sejam compatíveis com os aplicativos do MS-DOS.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe

```
change port [<PortX>=<PortY| /d <PortX| /query]
```

### <a name="parameters"></a>Parâmetros


|    Parâmetro    |              Descrição               |
|-----------------|----------------------------------------|
| <PortX>=<PortY> | Mapeia COM <*PortX*> para <*porta*> |
|   /d <PortX>    | Exclui o mapeamento para COM <*PortX*> |
|     /Query      | Exibe os mapeamentos de porta atuais |
|       /?        | Exibe a ajuda no prompt de comando |

## <a name="remarks"></a>Comentários

- A maioria dos aplicativos do MS-DOS dá suporte apenas a COM1 a COM4 portas seriais. O comando **Change Port** mapeia uma porta serial para um número de porta diferente, permitindo que os aplicativos que não dão suporte a portas com de alto número acessem a porta serial. o remapeamento funcionará somente para a sessão atual e não será mantido se você fizer logoff de uma sessão e depois fizer logon novamente.

- Use a **porta de alteração** sem parâmetros para exibir as portas com disponíveis e seus mapeamentos atuais.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

- Para mapear COM12 para COM1 para uso por um aplicativo baseado em MS-dos, digite:
  ```
  change port com12=com1
  ```
- Para exibir os mapeamentos de porta atuais, digite:
  ```
  change port /query
  ```

### <a name="additional-references"></a>Referências adicionais
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [change](change.md)
- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
