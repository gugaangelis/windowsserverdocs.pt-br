---
title: reset session
description: Artigo de referência para o comando Reset Session, que permite redefinir uma sessão em um Host da Sessão da Área de Trabalho Remota Server.
ms.topic: reference
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 745a3ba51714ad3f5431dedbe9cebedf77e4ae72
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626916"
---
# <a name="reset-session"></a>reset session

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que você redefina (exclua) uma sessão em um servidor Host da Sessão da Área de Trabalho Remota. Você deve redefinir uma sessão somente quando ela não está funcionando corretamente ou parece ter parado de responder.

> [!NOTE]
> Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
reset session {<sessionname> | <sessionID>} [/server:<servername>] [/v]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<sessionname>` | Especifica o nome da sessão que você deseja redefinir. Para determinar o nome da sessão, use o [comando Query Session](query-session.md). |
| `<sessionID>` | Especifica a ID da sessão a ser redefinida. |
| /server:`<servername>` | Especifica o servidor de terminal que contém a sessão que você deseja redefinir. Caso contrário, ele usa o servidor de Host da Sessão da Área de Trabalho Remota atual. Esse parâmetro será necessário apenas se você usar esse comando de um servidor remoto. |
| /v | Exibe informações sobre as ações que estão sendo executadas. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="remarks"></a>Comentários

- Você sempre pode redefinir suas próprias sessões, mas deve ter permissão de acesso **controle total** para redefinir a sessão de outro usuário. Lembre-se de que redefinir a sessão de um usuário sem aviso o usuário pode resultar na perda de dados na sessão.

## <a name="examples"></a>Exemplos

Para redefinir a sessão de *RDP designada-TCP # 6*, digite:

```
reset session rdp-tcp#6
```

Para redefinir a sessão que usa a *ID de sessão 3*, digite:

```
reset session 3
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Serviços de Área de Trabalho Remota referência de comando](remote-desktop-services-terminal-services-command-reference.md)
