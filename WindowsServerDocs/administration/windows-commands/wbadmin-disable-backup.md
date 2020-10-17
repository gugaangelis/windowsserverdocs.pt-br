---
title: wbadmin disable backup
description: Artigo de referência do comando Wbadmin Disable backup, que interrompe a execução dos backups diários agendados existentes.
ms.topic: reference
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f490ede3b6f48285291b1658458d8c8c176bb2c8
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156043"
---
# <a name="wbadmin-disable-backup"></a>wbadmin disable backup

Interrompe a execução dos backups diários agendados existentes.

Para desabilitar um backup diário agendado usando esse comando, você deve ser um membro do grupo **Administradores** ou deve ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

## <a name="syntax"></a>Sintaxe

```
wbadmin disable backup [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -quiet | Executa o comando sem prompts para o usuário. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin enable backup](wbadmin-enable-backup.md)