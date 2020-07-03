---
title: bitsadmin reset
description: Artigo de referência para o comando de redefinição de Bitsadmin, que cancela todos os trabalhos na fila de transferência de Propriedade do usuário atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc8faf10f991f06609d653c8cb7a1dc89de2fa8a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926394"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Cancela todos os trabalhos na fila de transferência de Propriedade do usuário atual. Não é possível redefinir trabalhos criados pelo sistema local. Em vez disso, você deve ser um administrador e usar o Agendador de tarefas para agendar esse comando como uma tarefa usando as credenciais do sistema local.

> [!NOTE]
> Se você tiver privilégios de administrador no BITSAdmin 1,5 e anteriores, a opção/Reset reiniciar cancelará todos os trabalhos na fila. Além disso, a opção/AllUsers não tem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| /allusers | Opcional. Cancela todos os trabalhos na fila de Propriedade do usuário atual. Você deve ter privilégios de administrador para usar esse parâmetro. |

## <a name="examples"></a>Exemplos

Para cancelar todos os trabalhos na fila de transferência para o usuário atual.

```
bitsadmin /reset
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
