---
title: eventcreate
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80d575364adbeba9d9ea4da75a0a866bcc02acea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818707"
---
# <a name="eventcreate"></a>eventcreate



Permite que um administrador crie um evento personalizado em um log de eventos especificado. Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<Computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u \<domínio \ usuário >|Executa o comando com as permissões de conta de usuário especificada por \<usuário > ou < domínio \ usuário >. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual.|
|/p \<Password>|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|/l {aplicativo\|sistema}|Especifica o nome do log de eventos onde o evento será criado. Os nomes de log válido são o aplicativo e do sistema.|
|/so \<SrcName>|Especifica a origem a ser usado para o evento. Uma fonte válida pode ser qualquer cadeia de caracteres e deve representar o aplicativo ou componente que está gerando o evento.|
|/t {erro\|aviso\|informações\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|Especifica o tipo de evento a ser criado. Os tipos válidos são ERROR, WARNING, informações, SUCCESSAUDIT e FAILUREAUDIT.|
|/id \<EventID>|Especifica a ID de evento para o evento. Uma ID válida é qualquer número de 1 a 1000.|
|/d \<descrição >|Especifica a descrição a ser usado para o evento recém-criado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Eventos personalizados não podem ser gravados no log de segurança.

## <a name="BKMK_examples"></a>Exemplos

Os exemplos a seguir mostram como você pode usar o comando eventcreate:
```
eventcreate /t error /id 100 /l application /d "Create event in application log"
eventcreate /t information /id 1000 /so winmgmt /d "Create event in WinMgmt source"
eventcreate /t error /id 2001 /so winword /l application /d "new src Winword in application log"
eventcreate /s server /t error /id 100 /l application /d "Remote machine without user credentials"
eventcreate /s server /u user /p password /id 100 /t error /l application /d "Remote machine with user credentials"
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d "Creating events on Multiple remote machines"
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d "Remote machine with partial user credentials"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
