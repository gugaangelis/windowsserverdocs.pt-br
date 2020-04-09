---
title: eventcreate
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79b10963abef9918e5962fdaf7d387a129873452
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845089"
---
# <a name="eventcreate"></a>eventcreate



Permite que um administrador crie um evento personalizado em um log de eventos especificado. Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<computador >|Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u \<domínio \ usuário >|Executa o comando com as permissões de conta do usuário especificado por \<usuário > ou < domínio \ usuário >. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.|
|/p \<senha >|Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .|
|/l {sistema de\|de aplicativos}|Especifica o nome do log de eventos em que o evento será criado. Os nomes de log válidos são aplicativo e sistema.|
|/so \<SrcName >|Especifica a origem a ser usada para o evento. Uma origem válida pode ser qualquer cadeia de caracteres e deve representar o aplicativo ou componente que está gerando o evento.|
|/t {erro\|aviso\|informações\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|Especifica o tipo de evento a ser criado. Os tipos válidos são erro, aviso, informações, SUCCESSAUDIT e FAILUREAUDIT.|
|/ID \<EventID >|Especifica a ID do evento para o evento. Uma ID válida é qualquer número de 1 a 1000.|
|/d \<Descrição >|Especifica a descrição a ser usada para o evento recém-criado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Eventos personalizados não podem ser gravados no log de segurança.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Os exemplos a seguir mostram como você pode usar o comando EventCreate:
```
eventcreate /t error /id 100 /l application /d Create event in application log
eventcreate /t information /id 1000 /so winmgmt /d Create event in WinMgmt source
eventcreate /t error /id 2001 /so winword /l application /d new src Winword in application log
eventcreate /s server /t error /id 100 /l application /d Remote machine without user credentials
eventcreate /s server /u user /p password /id 100 /t error /l application /d Remote machine with user credentials
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d Creating events on Multiple remote machines
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d Remote machine with partial user credentials
```

#### <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
