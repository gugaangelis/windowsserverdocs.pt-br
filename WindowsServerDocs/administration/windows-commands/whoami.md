---
title: whoami
description: Tópico de referência para whoami, que exibe informações de usuário, grupo e privilégios para o usuário que está conectado no momento ao sistema local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d672b3aaa20125c5c1da10fa3a5811fb5060d11
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725801"
---
# <a name="whoami"></a>whoami



Exibe informações de usuário, grupo e privilégios para o usuário que está conectado no momento ao sistema local. Se usado sem parâmetros, **whoami** exibirá o domínio atual e o nome de usuário.



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
|/Fo \<formato>|Especifica o formato de saída. Os valores válidos incluem:</br>**tabela** Exibe a saída em uma tabela. Este é o valor padrão.</br>**lista** de Exibe a saída em uma lista.</br>**CSV** Exibe a saída no formato de valores separados por vírgulas (CSV).|
|/all|Exibe todas as informações no token de acesso atual, incluindo o nome de usuário atual, identificadores de segurança (SID), privilégios e grupos aos quais o usuário atual pertence.|
|/NH|Especifica que o cabeçalho da coluna não deve ser exibido na saída. Isso é válido somente para formatos de tabela e CSV.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a>Exemplos

Para exibir o domínio e o nome de usuário da pessoa que está atualmente conectada a este computador, digite:
```
whoami
```
Saída semelhante à seguinte exibida:
```
DOMAIN1\administrator
```
Para exibir todas as informações no token de acesso atual, digite:
```
whoami /all
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)