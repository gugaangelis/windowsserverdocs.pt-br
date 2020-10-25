---
title: wbadmin stop job
description: Artigo de referência para o comando Wbadmin Stop Job, que cancela a operação de backup ou recuperação que está em execução no momento.
ms.topic: reference
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9952f28f674da6eae295dcffc0357cade19fdd1c
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524701"
---
# <a name="wbadmin-stop-job"></a>wbadmin stop job

Cancela a operação de backup ou recuperação que está em execução no momento.

> [!IMPORTANT]
> As operações canceladas não podem ser reiniciadas. Você deve executar um backup cancelado ou uma operação de recuperação desde o início.

Para interromper uma operação de backup ou recuperação usando esse comando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

## <a name="syntax"></a>Sintaxe

```
wbadmin stop job [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -quiet | Executa o comando sem prompts para o usuário. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)
