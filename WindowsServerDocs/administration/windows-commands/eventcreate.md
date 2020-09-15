---
title: eventcreate
description: Artigo de referência para o comando EventCreate, que permite que um administrador crie um evento personalizado em um log de eventos especificado.
ms.topic: reference
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d2fa1e4a90d0c24138c5353c5cf1b22308204d6f
ms.sourcegitcommit: 0b3d6661c44aa1a697087e644437279142726d84
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90083647"
---
# <a name="eventcreate"></a>eventcreate

Permite que um administrador crie um evento personalizado em um log de eventos especificado.

> [!IMPORTANT]
> Eventos personalizados não podem ser gravados no log de segurança.

## <a name="syntax"></a>Sintaxe

```
eventcreate [/s <computer> [/u <domain\user> [/p <password>]] {[/l {APPLICATION|SYSTEM}]|[/so <srcname>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <eventID> /d <description>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| /u `<domain\user>` | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain\user>` . O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| /p `<password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| /l `{APPLICATION | SYSTEM}` | Especifica o nome do log de eventos em que o evento será criado. Os nomes de log válidos são **aplicativo** ou **sistema**. |
| /so `<srcname>` | Especifica a origem a ser usada para o evento. Uma origem válida pode ser qualquer cadeia de caracteres e deve representar o aplicativo ou componente que está gerando o evento. |
| /t `{ERROR | WARNING | INFORMATION | SUCCESSAUDIT | FAILUREAUDIT}` | Especifica o tipo de evento a ser criado. Os tipos válidos são **erro**, **aviso**, **informações**, **SuccessAudit**e **FailureAudit**. |
| /ID `<eventID>` | Especifica a ID do evento para o evento. Uma ID válida é qualquer número de 1 a 1000. |
| /d `<description>` | Especifica a descrição a ser usada para o evento recém-criado. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Os exemplos a seguir mostram como você pode usar o comando **EventCreate** :

```
eventcreate /t ERROR /id 100 /l application /d "Create event in application log"
eventcreate /t INFORMATION /id 1000 /d "Create event in WinMgmt source"
eventcreate /t ERROR /id 201 /so winword /l application /d "New src Winword in application log"
eventcreate /s server /t ERROR /id 100 /l application /d "Remote machine without user credentials"
eventcreate /s server /u user /p password /id 100 /t ERROR /l application /d "Remote machine with user credentials"
eventcreate /s server1 /s server2 /u user /p password /id 100 /t ERROR /d "Creating events on Multiple remote machines"
eventcreate /s server /u user /id 100 /t WARNING /d "Remote machine with partial user credentials"
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
