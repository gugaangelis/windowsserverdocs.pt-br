---
title: msg
description: Artigo de referência para o comando MSG, que envia uma mensagem para um usuário em um Host da Sessão da Área de Trabalho Remota Server
ms.topic: reference
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 34b51bb82fdac6b847d69b4d59a345777054839f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639631"
---
# <a name="msg"></a>msg

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia uma mensagem para um usuário em um servidor de Host da Sessão da Área de Trabalho Remota.

> [!NOTE]
> Você deve ter permissão de acesso especial à mensagem para enviar uma mensagem.

## <a name="syntax"></a>Sintaxe

```
msg {<username> | <sessionname> | <sessionID>| @<filename> | *} [/server:<servername>] [/time:<seconds>] [/v] [/w] [<message>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<username>` | Especifica o nome do usuário para o qual você deseja receber a mensagem. Se você não especificar um usuário ou uma sessão, esse comando exibirá uma mensagem de erro. Ao especificar uma sessão, ela deve ser uma ativa. |
| `<sessionname>` | Especifica o nome da sessão na qual você deseja receber a mensagem. Se você não especificar um usuário ou uma sessão, esse comando exibirá uma mensagem de erro. Ao especificar uma sessão, ela deve ser uma ativa. |
| `<sessionID>` | Especifica a ID numérica da sessão cujo usuário você deseja receber uma mensagem. |
| `@<filename>` | Identifica um arquivo que contém uma lista de nomes de usuário, nomes de sessão e IDs de sessão que você deseja receber a mensagem. |
| * | Envia a mensagem a todos os nomes de usuário no sistema. |
| /server:`<servername>` | Especifica o servidor de Host da Sessão da Área de Trabalho Remota cuja sessão ou usuário você deseja receber a mensagem. Se não for especificado, o **/Server** usará o servidor no qual você está conectado no momento. |
| momento`<seconds>` | Especifica a quantidade de tempo que a mensagem enviada é exibida na tela do usuário. Depois que o limite de tempo é atingido, a mensagem desaparece. Se nenhum limite de tempo for definido, a mensagem permanecerá na tela do usuário até que o usuário veja a mensagem e clique em **OK**. |
| /v | Exibe informações sobre as ações que estão sendo executadas. |
| /w | Aguarda uma confirmação do usuário de que a mensagem foi recebida. Use esse parâmetro com `/time:<*seconds*>` para evitar um possível atraso longo se o usuário não responder imediatamente. O uso desse parâmetro com **/v** também é útil. |
| `<message>` | Especifica o texto da mensagem que você deseja enviar. Se nenhuma mensagem for especificada, você será solicitado a inserir uma mensagem. Para enviar uma mensagem contida em um arquivo, digite o símbolo menor que (<) seguido pelo nome do arquivo. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para enviar uma mensagem com o direito, *vamos atender às às 13:00 hoje* para todas as sessões do *Usuário1*, digite:

```
msg User1 Let's meet at 1PM today
```

Para enviar a mesma mensagem para a sessão *modeM02*, digite:

```
msg modem02 Let's meet at 1PM today
```

Para enviar a mensagem a todas as sessões contidas na *lista*de userfile, digite:

```
msg @userlist Let's meet at 1PM today
```

Para enviar a mensagem a todos os usuários que estão conectados, digite:

```
msg * Let's meet at 1PM today
```

Para enviar a mensagem a todos os usuários, com um tempo limite de confirmação (por exemplo, 10 segundos), digite:

```
msg * /time:10 Let's meet at 1PM today
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
