---
title: telnet
description: Artigo de referência para o comando telnet, que se comunica com um computador que executa o serviço do servidor Telnet.
ms.topic: reference
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cf4fa5754aec18662800f4536afd16a427ca0952
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717923"
---
# <a name="telnet"></a>telnet

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comunica-se com um computador que executa o serviço do servidor Telnet. Executar esse comando sem parâmetros permite que você insira o contexto de Telnet, conforme indicado pelo prompt Telnet (**Microsoft telnet>**). No prompt de Telnet, você pode usar comandos Telnet para gerenciar o computador que executa o cliente Telnet.

> [!IMPORTANT]
> Você deve instalar o software cliente Telnet antes de poder executar este comando. Para obter mais informações, consulte [instalando o Telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293(v=ws.10)).

## <a name="syntax"></a>Sintaxe

```
telnet [/a] [/e <escapechar>] [/f <filename>] [/l <username>] [/t {vt100 | vt52 | ansi | vtnt}] [<host> [<port>]] [/?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /a | Tenta o logon automático. Mesmo que a opção **/l** , exceto pelo fato de que ela usa o nome do usuário conectado no momento. |
| /e `<escapechar>` | Especifica o caractere de escape usado para entrar no prompt do cliente Telnet. |
| /f `<filename>` | Especifica o nome do arquivo usado para o log do lado do cliente. |
| /l `<username>` | Especifica o nome de usuário no qual fazer logon no computador remoto. |
| /t `{vt100 | vt52 | ansi | vtnt}` | Especifica o tipo de terminal. Os tipos de terminal com suporte são **vt100**, **vt52**, **ANSI**e **VTNT**. |
| `<host> [<port>]` | Especifica o nome do host ou endereço IP do computador remoto ao qual se conectar e, opcionalmente, a porta TCP a ser usada (o padrão é a porta TCP 23). |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para usar o Telnet para se conectar ao computador que executa o serviço do servidor Telnet em *Telnet.Microsoft.com*, digite:

```
telnet telnet.microsoft.com
```

Para usar o Telnet para se conectar ao computador que executa o serviço do servidor Telnet em *Telnet.Microsoft.com na porta TCP 44* e ro Registre a atividade de sessão em um arquivo local chamado *telnetlog.txt*, digite:

```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Instalando o Telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293(v=ws.10))

- [Referência técnica de Telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754987(v=ws.10))
