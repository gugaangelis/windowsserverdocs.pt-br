---
title: lista de Auditpol
description: Os comandos do Windows tópico para **lista** de buscas – lista categorias de política de auditoria e/ou subcategorias ou lista os usuários para os quais uma política de auditoria por usuário é definida.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27a89ae18838989b4f2df27d777c1c35249b8991
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382456"
---
# <a name="auditpol-list"></a>lista de Auditpol

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

lista categorias de política de auditoria e/ou subcategorias ou lista os usuários para os quais uma política de auditoria por usuário é definida.

## <a name="syntax"></a>Sintaxe
```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/|Recupera todos os usuários para os quais a política de auditoria por usuário foi definida. Se usado com o parâmetro/v, o SID (identificador de segurança) do usuário também será exibido.|
|/Category|Exibe os nomes das categorias compreendidas pelo sistema. Se for usado com o parâmetro/v, o GUID (identificador global exclusivo) da categoria também será exibido.|
|/subcategory|Exibe os nomes das subcategorias e do GUID associado.|
|/v|Exibe o GUID com a categoria ou subcategoria, ou quando usado com/user, exibe o SID de cada usuário.|
|/r|Exibe a saída como um relatório no formato de valores separados por vírgulas (CSV).|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
para todas as operações de lista para a política por usuário, você deve ter permissão de leitura nesse objeto definido no descritor de segurança. Você também pode executar operações de lista por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação de lista.
## <a name="BKMK_examples"></a>Disso
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
auditpol /list /subcategory:"detailed Tracking","DS Access"
```
#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
