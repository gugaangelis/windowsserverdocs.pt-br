---
title: tscon
description: Artigo de referência para tscon, que se conecta a outra sessão em um Host da Sessão da Área de Trabalho Remota Server.
ms.topic: reference
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 17b18aea265ff7c703c2ef6c9c3d0021a9d9ea00
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156395"
---
# <a name="tscon"></a>tscon

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se a outra sessão em um servidor Host da Sessão da Área de Trabalho Remota.

> [!IMPORTANT]
> Você deve ter permissão de **acesso controle total** ou conectar permissão de **acesso especial** para se conectar a outra sessão.

> [!NOTE]
> Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
tscon {<sessionID> | <sessionname>} [/dest:<sessionname>] [/password:<pw> | /password:*] [/v]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<sessionID>` | Especifica a ID da sessão à qual você deseja se conectar. Se você usar o `/dest:<sessionname>` parâmetro opcional, também poderá especificar o nome da sessão atual. |
| `<sessionname>` | Especifica o nome da sessão à qual você deseja se conectar. |
| /dest`<sessionname>` | Especifica o nome da sessão atual. Esta sessão será desconectada quando você se conectar à nova sessão. Você também pode usar esse parâmetro para conectar a sessão de outro usuário a uma sessão diferente. |
| /Password`<pw>` | Especifica a senha do usuário que possui a sessão à qual você deseja se conectar. Essa senha é necessária quando o usuário que está se conectando não possui a sessão. |
| /Password`*` | Solicita a senha do usuário que possui a sessão à qual você deseja se conectar. |
| /v | Exibe informações sobre as ações que estão sendo executadas. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Esse comando falhará se você não especificar uma senha no parâmetro **/password** e a sessão de destino pertencer a um usuário diferente do atual.

- Não é possível conectar-se à sessão do console.

## <a name="examples"></a>Exemplos

Para se conectar à *sessão 12* no servidor host da sessão de serviços de área de trabalho remota atual e para desconectar a sessão atual, digite:

```
tscon 12
```

Para se conectar à *sessão 23* no servidor host da sessão de serviços de área de trabalho remota atual usando a senha *MyPASS*e para desconectar a sessão atual, digite:

```
tscon 23 /password:mypass
```

Para conectar a sessão chamada *TERM03* à sessão chamada *TERM05*e, em seguida, para desconectar a sessão *TERM05*, digite:

```
tscon TERM03 /v /dest:TERM05
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
