---
title: auditpol list
description: Artigo de referência do comando lista de Auditpol, que lista categorias de política de auditoria e subcategorias, ou lista os usuários para os quais uma política de auditoria por usuário é definida.
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5047708056e4e926dc917b80b4b0a41ce5f9d773
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895422"
---
# <a name="auditpol-list"></a>auditpol list

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lista categorias de política de auditoria e subcategorias ou lista os usuários para os quais uma política de auditoria por usuário é definida.

Para executar operações de *lista* na política *por usuário* , você deve ter permissão de **leitura** para esse objeto definido no descritor de segurança. Você também pode executar operações de *lista* se tiver o direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar as operações de *lista* geral.

## <a name="syntax"></a>Sintaxe

```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| / | Recupera todos os usuários para os quais a política de auditoria por usuário foi definida. Se usado com o parâmetro/v, o SID (identificador de segurança) do usuário também será exibido. |
| /category | Exibe os nomes das categorias compreendidas pelo sistema. Se for usado com o parâmetro/v, o GUID (identificador global exclusivo) da categoria também será exibido. |
| /subcategory | Exibe os nomes das subcategorias e do GUID associado. |
| /v | Exibe o GUID com a categoria ou subcategoria, ou quando usado com/user, exibe o SID de cada usuário. |
| /r | Exibe a saída como um relatório no formato de valores separados por vírgulas (CSV). |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para listar todos os usuários que têm uma política de auditoria definida, digite:

```
auditpol /list /user
```

Para listar todos os usuários que têm uma política de auditoria definida e seu SID associado, digite:

```
auditpol /list /user /v
```

Para listar todas as categorias e subcategorias no formato de relatório, digite:

```
auditpol /list /subcategory:* /r
```

Para listar as subcategorias das categorias controle detalhado e acesso DS, digite:

```
auditpol /list /subcategory:detailed Tracking,DS Access
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
