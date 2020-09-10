---
title: wbadmin disable backup
description: Artigo de referência para WBADMIN Disable backup, que interrompe a execução dos backups diários agendados existentes.
ms.topic: reference
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5525dc4c62900f2f250fc36670f83cefd0b55e35
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640760"
---
# <a name="wbadmin-disable-backup"></a>wbadmin disable backup



Interrompe a execução dos backups diários agendados existentes.

Para desabilitar um backup diário agendado, você deve ser um membro do grupo **Administradores** ou deve ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando** e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin disable backup
[-quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)