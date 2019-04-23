---
title: bitsadmin reset
description: Tópico de comandos do Windows para **bitsadmin redefinir** -cancela todos os trabalhos na fila de transferência que o usuário atual possui.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7c29aac55393cd87145583814b3ffa8f0a2c3b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874247"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Cancela todos os trabalhos na fila de transferência que o usuário atual possui.

**BITSAdmin 1.5 e anterior**: Se você tiver privilégios de administrador **redefina** cancela todos os trabalhos na fila. Não há suporte para a opção /AllUsers.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|AllUsers|Opcional — cancela todos os trabalhos na fila.|

## <a name="remarks"></a>Comentários

Você deve ter privilégios de administrador para usar o **AllUsers** parâmetro.

> [!NOTE]
> Os administradores não é possível redefinir os trabalhos criados pelo sistema Local. Use o Agendador de tarefas para agendar esse comando como uma tarefa usando as credenciais do sistema Local.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir cancela todos os trabalhos na fila de transferência para o usuário atual.
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)