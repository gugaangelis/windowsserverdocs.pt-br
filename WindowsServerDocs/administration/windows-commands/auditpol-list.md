---
title: lista de Auditpol
description: Tópico de comandos do Windows para **auditpol lista** – categorias de política e/ou subcategorias de auditoria de listas ou lista de usuários para os quais a política de auditoria por usuário é definido.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 08f524ef0aacd731f709ce7a2e17b3d831da1e5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858577"
---
# <a name="auditpol-list"></a>lista de Auditpol

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

listas de lista de usuários para os quais uma política de auditoria por usuário é definida e/ou subcategorias ou categorias de política de auditoria.

## <a name="syntax"></a>Sintaxe
```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/user|Recupera todos os usuários para os quais a política de auditoria por usuário foi definida. Se usado com o parâmetro /v, o identificador de segurança (SID) do usuário também é exibido.|
|/category|Exibe os nomes das categorias compreendidos pelo sistema. Se usado com o parâmetro /v, a categoria globalmente exclusivo (GUID) também é exibida.|
|/subcategory|Exibe os nomes das subcategorias e sua GUID associada.|
|/v|Exibe o GUID com a categoria ou subcategoria, ou quando usado com /user, o SID de cada usuário.|
|/r|Exibe a saída como um relatório no formato de valores separados por vírgulas (CSV).|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
todas as operações de lista para a política por usuário, você deve ter permissão de leitura naquele objeto definido no descritor de segurança. Você também pode executar operações de lista que possui o **gerenciar o log de auditoria e segurança** direito de usuário (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar a operação de lista.
## <a name="BKMK_examples"></a>Exemplos
Para listar todos os usuários que têm uma política de auditoria definido, digite:
```
auditpol /list /user
```
Para listar todos os usuários que têm uma política de auditoria definido e seu SID associado, digite:
```
auditpol /list /user /v
```
Para listar todas as categorias e subcategorias no formato de relatório, digite:
```
auditpol /list /subcategory:* /r
```
Para listar as subcategorias das categorias detalhadas de rastreamento e o acesso ao DS, digite:
```
auditpol /list /subcategory:"detailed Tracking","DS Access"
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
