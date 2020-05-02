---
title: cmdkey
description: Tópico de referência para o comando cmdkey, que cria, lista e exclui nomes de usuário e senhas ou credenciais armazenadas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4005707785101fcc1fb0030ffe895668bd65f730
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712161"
---
# <a name="cmdkey"></a>cmdkey

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria, lista e exclui nomes de usuário e senhas ou credenciais armazenados.

## <a name="syntax"></a>Sintaxe

```
cmdkey [{/add:<targetname>|/generic:<targetname>}] {/smartcard | /user:<username> [/pass:<password>]} [/delete{:<targetname> | /ras}] /list:<targetname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| /Add`<targetname>` | Adiciona um nome de usuário e senha à lista.<p>Requer o parâmetro de `<targetname>` que identifica o computador ou o nome de domínio ao qual esta entrada será associada. |
| /genérico`<targetname>` | Adiciona credenciais genéricas à lista.<p>Requer o parâmetro de `<targetname>` que identifica o computador ou o nome de domínio ao qual esta entrada será associada. |
| /smartcard | Recupera a credencial de um cartão inteligente. Se mais de um cartão inteligente for encontrado no sistema quando essa opção for usada, **cmdkey** exibirá informações sobre todos os cartões inteligentes disponíveis e, em seguida, solicitará que o usuário especifique qual deles usar. |
| /`<username>` | Especifica o nome do usuário ou da conta a ser armazenada com essa entrada. Se `<username>` não for fornecido, ele será solicitado. |
|passá`<password>` | Especifica a senha a ser armazenada com essa entrada. Se `<password>` não for fornecido, ele será solicitado. As senhas não são exibidas depois que são armazenadas. |
| /delete{:`<targetname>` | Services | Exclui um nome de usuário e uma senha da lista. Se `<targetname>` for especificado, essa entrada será excluída. Se `/ras` for especificado, a entrada de acesso remoto armazenada será excluída. |
| /List`<targetname>` | Exibe a lista de nomes de usuário e credenciais armazenados. Se `<targetname>` não for especificado, todos os nomes de usuário e credenciais armazenados serão listados. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para exibir uma lista de todos os nomes de usuário e credenciais que estão armazenados, digite:

```
cmdkey /list
```

Para adicionar um nome de usuário e uma senha para o usuário *Mikedan* acessar o computador *Server01* com a senha *Kleo*, digite:

```
cmdkey /add:server01 /user:mikedan /pass:Kleo
```

Para adicionar um nome de usuário e uma senha para o usuário *Mikedan* acessar o computador *Server01* e solicitar a senha sempre que Server01 for acessado, digite:

```
cmdkey /add:server01 /user:mikedan
```

Para excluir uma credencial armazenada pelo acesso remoto, digite:

```
cmdkey /delete /ras
```

Para excluir uma credencial armazenada para *SERVER01*, digite:

```
cmdkey /delete:server01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
