---
title: whoami
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6844ba001c2ebd7407b77f97204069a48a1b595b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840147"
---
# <a name="whoami"></a>whoami



Exibe informações de usuário, grupo e privilégios do usuário que está conectado no momento ao sistema local. Se usado sem parâmetros, **whoami** exibe o nome de usuário e domínio atual.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/upn|Exibe o nome de usuário no formato de nome principal (UPN) do usuário.|
|/fqdn|Exibe o nome de usuário no formato de nome (FQDN) de domínio totalmente qualificado.|
|/logonid|Exibe a ID de logon do usuário atual.|
|/user|Exibe o nome de usuário e domínio atual e o identificador de segurança (SID).|
|/groups|Exibe os grupos de usuários aos quais o usuário atual pertence.|
|/priv|Exibe os privilégios de segurança do usuário atual.|
|/FO \<formato >|Especifica o formato de saída. Os valores válidos incluem:</br>**tabela** exibe a saída em uma tabela. Este é o valor padrão.</br>**lista** exibe a saída em uma lista.</br>**CSV** exibe a saída no formato de valores separados por vírgulas (CSV).|
|/all|Exibe todas as informações no token de acesso atual, incluindo o nome de usuário atual, identificadores de segurança (SID), privilégios e grupos aos quais o usuário atual pertence.|
|/nh|Especifica que o cabeçalho de coluna não deve ser exibido na saída. Isso é válido apenas para formatos CSV e de tabela.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Exemplos

Para exibir o nome de usuário e domínio da pessoa que está conectado no momento a este computador, digite:
```
whoami
```
Saída semelhante à seguinte será exibida:
```
DOMAIN1\administrator
```
Para exibir todas as informações no token de acesso atual, digite:
```
whoami /all
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)