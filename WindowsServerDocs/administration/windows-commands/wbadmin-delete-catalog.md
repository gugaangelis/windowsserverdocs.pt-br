---
title: wbadmin delete catalog
description: Artigo de referência para o comando Wbadmin Delete Catalog, que exclui o catálogo de backup armazenado no computador local.
ms.topic: reference
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4a19d856b65fdcf17fb369d81607445530f77275
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156108"
---
# <a name="wbadmin-delete-catalog"></a>wbadmin delete catalog

Exclui o catálogo de backup que está armazenado no computador local. Use este comando quando o catálogo de backup tiver sido corrompido e você não puder restaurá-lo usando o comando [Wbadmin restore catalog](wbadmin-restore-catalog.md) .

Para excluir um catálogo de backup usando este comando, você deve ser um membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

## <a name="syntax"></a>Sintaxe

```
wbadmin delete catalog [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -quiet | Executa o comando sem prompts para o usuário. |

#### <a name="remarks"></a>Comentários

- Se você excluir o catálogo de backup de um computador, não poderá mais acessar os backups criados para esse computador usando o snap-in Backup do Windows Server. No entanto, se você puder acessar outro local de backup e executar o comando [Wbadmin restore catalog](wbadmin-restore-catalog.md) , poderá restaurar o catálogo de backup desse local.

- É altamente recomendável que você crie um novo backup depois de excluir um catálogo de backup.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin restore catalog](wbadmin-restore-catalog.md)

- [Remove-WBCatalog](/powershell/module/windowsserverbackup/Remove-WBCatalog)
