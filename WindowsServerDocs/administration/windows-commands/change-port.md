---
title: change port
description: Artigo de referência para o comando Change Port, que lista ou altera os mapeamentos de porta COM para que sejam compatíveis com os aplicativos do MS-DOS.
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a38f4f4885ac13c40a7e2a340bf94623bcbdd77d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629897"
---
# <a name="change-port"></a>change port

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lista ou altera os mapeamentos de porta COM para que sejam compatíveis com os aplicativos do MS-DOS.

> [!NOTE]
> Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
change port [<portX>=<portY| /d <portX | /query]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|-----------------|----------------------------------------|
| <portX>=<portY> | Mapeia COM `<*portX*>` para `<*portY*>` |
| /d <portX> | Exclui o mapeamento para COM `<*portX*>` |
| /Query | Exibe os mapeamentos de porta atuais. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- A maioria dos aplicativos do MS-DOS dá suporte apenas a COM1 a COM4 portas seriais. O comando **Change Port** mapeia uma porta serial para um número de porta diferente, permitindo que os aplicativos que não dão suporte a portas com de alto número acessem a porta serial. O remapeamento funcionará somente para a sessão atual e não será mantido se você fizer logoff de uma sessão e depois fizer logon novamente.

- Use a **porta de alteração** sem parâmetros para exibir as portas com disponíveis e seus mapeamentos atuais.

## <a name="examples"></a>Exemplos

- Para mapear COM12 para COM1 para uso por um aplicativo baseado em MS-dos, digite:

  ```
  change port com12=com1
  ```

- Para exibir os mapeamentos de porta atuais, digite:

  ```
  change port /query
  ```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [alterar comando](change.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
