---
title: bitsadmin reset
description: O tópico de comandos do Windows para **Bitsadmin Reset**, que cancela todos os trabalhos na fila de transferência de Propriedade do usuário atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed1dcf9bce06af527ffb5b6a79d76d860d78450c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849789"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Cancela todos os trabalhos na fila de transferência de Propriedade do usuário atual. > Não é possível redefinir os trabalhos criados pelo sistema local. Em vez disso, você deve ser um administrador e usar o Agendador de tarefas para agendar esse comando como uma tarefa usando as credenciais do sistema local.

> [!NOTE]
> No BITSAdmin 1,5 e versões anteriores, se você tiver privilégios de administrador, a opção/Reset reiniciar cancelará todos os trabalhos na fila. Além disso, a opção/AllUsers não tem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| /allusers | Opcional. Cancela todos os trabalhos na fila de Propriedade do usuário atual. Você deve ter privilégios de administrador para usar esse parâmetro. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir cancela todos os trabalhos na fila de transferência para o usuário atual.

```
C:\>bitsadmin /reset
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)