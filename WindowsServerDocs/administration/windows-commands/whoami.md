---
title: whoami
description: Artigo de referência para whoami, que exibe informações de usuário, grupo e privilégios para o usuário que está conectado no momento ao sistema local.
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bdcef4072fe692f2717fe79814af926a2c151636
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896498"
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
|/Fo\<Format>|Especifica o formato de saída. Os valores válidos incluem:</br>**tabela** Exibe a saída em uma tabela. Este é o valor padrão.</br>**lista** de Exibe a saída em uma lista.</br>**CSV** Exibe a saída no formato de valores separados por vírgulas (CSV).|
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