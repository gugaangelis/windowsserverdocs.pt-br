---
title: whoami
description: O tópico de comandos do Windows para whoami, que exibe informações de usuário, grupo e privilégios para o usuário que está conectado no momento ao sistema local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ff45ed95b35215859f2f83aec75b33570ef46d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829269"
---
# <a name="whoami"></a>whoami



Exibe informações de usuário, grupo e privilégios para o usuário que está conectado no momento ao sistema local. Se usado sem parâmetros, **whoami** exibirá o domínio atual e o nome de usuário.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/upn|Exibe o nome de usuário no formato UPN (nome principal do usuário).|
|/fqdn|Exibe o nome de usuário no formato FQDN (nome de domínio totalmente qualificado).|
|/logonid|Exibe a ID de logon do usuário atual.|
|/|Exibe o domínio atual e o nome de usuário e o SID (identificador de segurança).|
|/groups|Exibe os grupos de usuários aos quais o usuário atual pertence.|
|/priv|Exibe os privilégios de segurança do usuário atual.|
|/Fo \<formato >|Especifica o formato de saída. Os valores válidos incluem:</br>**tabela** Exibe a saída em uma tabela. Esse é o valor padrão.</br>**lista** de Exibe a saída em uma lista.</br>**CSV** Exibe a saída no formato de valores separados por vírgulas (CSV).|
|/all|Exibe todas as informações no token de acesso atual, incluindo o nome de usuário atual, identificadores de segurança (SID), privilégios e grupos aos quais o usuário atual pertence.|
|/NH|Especifica que o cabeçalho da coluna não deve ser exibido na saída. Isso é válido somente para formatos de tabela e CSV.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir o domínio e o nome de usuário da pessoa que está atualmente conectada a este computador, digite:
```
whoami
```
Uma saída semelhante à seguinte é exibida:
```
DOMAIN1\administrator
```
Para exibir todas as informações no token de acesso atual, digite:
```
whoami /all
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)