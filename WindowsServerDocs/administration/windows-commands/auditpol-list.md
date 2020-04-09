---
title: lista de Auditpol
description: O tópico de comandos do Windows para a **lista Auditpol**, que lista categorias de política de auditoria e subcategorias, ou lista os usuários para os quais uma política de auditoria por usuário é definida.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e0aff46e62ea4e4259360b78aae223dfcd66ef7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851179"
---
# <a name="auditpol-list"></a>lista de Auditpol

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lista categorias de política de auditoria e/ou subcategorias ou lista os usuários para os quais uma política de auditoria por usuário é definida.

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

## <a name="remarks"></a>Comentários

Para todas as operações de lista para a política por usuário, você deve ter permissão de leitura nesse objeto definido no descritor de segurança. Você também pode executar operações de lista por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação de lista.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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
