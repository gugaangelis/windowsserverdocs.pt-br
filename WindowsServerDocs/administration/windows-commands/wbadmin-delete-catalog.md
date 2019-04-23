---
title: Catálogo de exclusão de WBADMIN
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d9f60cf79fcd856fa972d8f43f195c5823e5d81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841407"
---
# <a name="wbadmin-delete-catalog"></a>Catálogo de exclusão de WBADMIN



Exclui o catálogo de backup é armazenado no computador local. Use esse comando quando o catálogo de backup foi corrompido e não é possível restaurá-lo usando **wbadmin restauração catálogo**.

Para excluir um catálogo de backup com este subcomando, você deve ser um membro do **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando**e, em seguida, clique em **executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin delete catalog
[-quiet]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="remarks"></a>Comentários

Se você excluir o catálogo de backup para um computador, você não poderá acessar os backups criados do computador usando o snap-in Backup do Windows Server. Nesse caso, se você puder acessar outro local de backup, use **wbadmin restauração catálogo** para restaurar o catálogo de backup nesse local. Você deve criar um novo backup depois que o catálogo de backup é excluído.

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)