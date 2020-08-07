---
title: qwinsta
description: Artigo de referência para o comando Qwinsta, que exibe informações sobre sessões em um servidor Host da Sessão da Área de Trabalho Remota.
ms.topic: article
ms.assetid: a793212a-7ecd-44cb-a77b-c5c2edb34979
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 496088708d980ec2ad3253dc29aeec99b29a6d41
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884369"
---
# <a name="qwinsta"></a>qwinsta

Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre sessões em um servidor Host da Sessão da Área de Trabalho Remota. A lista inclui informações não apenas sobre sessões ativas, mas também sobre outras sessões que o servidor executa.

> [!NOTE]
> Esse comando é o mesmo que o [comando de sessão de consulta](query-session.md). Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
qwinsta [<sessionname> | <username> | <sessionID>] [/server:<servername>] [/mode] [/flow] [/connect] [/counter]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<sessionname>` | Especifica o nome da sessão que você deseja consultar. |
| `<username>` | Especifica o nome do usuário cujas sessões você deseja consultar. |
| `<sessionID>` | Especifica a ID da sessão que você deseja consultar. |
| /server:`<servername>` | Identifica o servidor host da sessão da área de trabalho remota a ser consultado. O padrão é o servidor atual. |
| /Mode | Exibe as configurações de linha atuais. |
| /flow | Exibe as configurações de controle de fluxo atuais. |
| /connect | Exibe as configurações de conexão atuais. |
| /Counter | Exibe informações de contadores atuais, incluindo o número total de sessões criadas, desconectadas e reconectadas. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Um usuário sempre pode consultar a sessão à qual o usuário está conectado no momento. Para consultar outras sessões, o usuário deve ter permissão de acesso especial.

- Se você não especificar uma sessão usando o <*nome de usuário*>, <*SessionName*> ou parâmetros *SessionID* , essa consulta exibirá informações sobre todas as sessões ativas no sistema.

- Quando **Qwinsta** retorna informações, um símbolo maior que `(>)` é exibido antes da sessão atual. Por exemplo:

    ```
    C:\>qwinsta
        SESSIONNAME     USERNAME        ID STATE    TYPE    DEVICE
        console         Administrator1  0 active    wdcon
        >rdp-tcp#1      User1           1 active    wdtshare
        rdp-tcp                         2 listen    wdtshare
                                        4 idle
                                        5 idle
    ```

    Em que:
  - **SessionName** especifica o nome atribuído à sessão.
  - **Username** indica o nome de usuário do usuário conectado à sessão.
  - O **estado** fornece informações sobre o estado atual da sessão.
  - **Tipo** indica o tipo de sessão.
  - O **dispositivo**, que não está presente no console ou nas sessões conectadas à rede, é o nome do dispositivo atribuído à sessão.
  - Todas as sessões em que o estado inicial é configurado como DESABILITAdo não aparecerão na lista **Qwinsta** até que sejam habilitadas.

### <a name="examples"></a>Exemplos

Para exibir informações sobre todas as sessões ativas no servidor *Servidor2*, digite:

```
qwinsta /server:Server2
```

Para exibir informações sobre a sessão ativa *modeM02*, digite:

```
qwinsta modeM02
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de sessão de consulta](query-session.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
