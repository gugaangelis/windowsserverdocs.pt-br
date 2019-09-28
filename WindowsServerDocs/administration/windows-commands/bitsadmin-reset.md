---
title: bitsadmin reset
description: O tópico de comandos do Windows para **Bitsadmin Reset** – cancela todos os trabalhos na fila de transferência que o usuário atual possui.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: adc6b07a7b5d1414c733fe6a3ac05eba7cb3029e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380815"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Cancela todos os trabalhos na fila de transferência que o usuário atual possui.

**BITSAdmin 1,5 e anterior**: Se você tiver privilégios de administrador, **redefina** cancels todos os trabalhos na fila. Não há suporte para a opção/AllUsers.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|AllUsers|Opcional – cancela todos os trabalhos na fila.|

## <a name="remarks"></a>Comentários

Você deve ter privilégios de administrador para usar o parâmetro **AllUsers** .

> [!NOTE]
> Os administradores não podem redefinir trabalhos criados pelo sistema local. Use o Agendador de tarefas para agendar esse comando como uma tarefa usando as credenciais do sistema local.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir cancela todos os trabalhos na fila de transferência para o usuário atual.
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)