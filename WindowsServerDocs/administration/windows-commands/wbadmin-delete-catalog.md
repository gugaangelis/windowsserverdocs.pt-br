---
title: Wbadmin excluir catálogo
description: O tópico de comandos do Windows para WBADMIN Delete Catalog, que exclui o catálogo de backup armazenado no computador local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5cf069163cb18c1763de2842b518f269b9fa57dd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829899"
---
# <a name="wbadmin-delete-catalog"></a>Wbadmin excluir catálogo



Exclui o catálogo de backup que está armazenado no computador local. Use este comando quando o catálogo de backup tiver sido corrompido e você não puder restaurá-lo usando **Wbadmin restore catalog**.

Para excluir um catálogo de backup com este subcomando, você deve ser um membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin delete catalog
[-quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="remarks"></a>Comentários

Se você excluir o catálogo de backup de um computador, não poderá acessar os backups criados desse computador usando o snap-in Backup do Windows Server. Nesse caso, se você puder acessar outro local de backup, use **Wbadmin restore catalog** para restaurar o catálogo de backup desse local. Você deve criar um novo backup depois que o catálogo de backup for excluído.

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)