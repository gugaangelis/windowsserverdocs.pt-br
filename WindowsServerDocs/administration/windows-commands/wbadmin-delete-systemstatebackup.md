---
title: wbadmin delete systemstatebackup
description: Artigo de referência para WBADMIN Delete systemstatebackup, que exclui os backups de estado do sistema que você especificar.
ms.topic: reference
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c54fa768251ecc450e36e65bc845067c3b96115b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89022930"
---
# <a name="wbadmin-delete-systemstatebackup"></a>wbadmin delete systemstatebackup


Exclui os backups de estado do sistema que você especificar. Se o volume especificado contiver backups diferentes dos backups de estado do sistema do servidor local, esses backups não serão excluídos.

> [!NOTE]
> Backup do Windows Server não faz backup ou recupera hives de usuário do registro (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou da recuperação do estado do sistema.

Para excluir um backup de estado do sistema com este subcomando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)


## <a name="syntax"></a>Sintaxe

```
wbadmin delete systemstatebackup
{-keepVersions:<NumberofCopies> | -version:<VersionIdentifier> | -deleteOldest}
[-backupTarget:<VolumeName>]
[-machine:<BackupMachineName>]
[-quiet]
```

> [!IMPORTANT]
> Um e apenas um desses parâmetros devem ser especificados: **-keepVersions**, **-version**ou **-deleteOldest**.

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-keepVersions|Especifica o número de backups de estado do sistema mais recentes a serem mantidos. O valor deve ser um inteiro positivo. O valor do parâmetro **-keepVersions: 0** exclui todos os backups de estado do sistema.|
|-version|Especifica o identificador de versão do backup no formato MM/DD/AAAA-HH: MM. Se você não souber o identificador de versão, digite **Wbadmin obter versões**.</br>Versões que são exclusivamente backups de estado do sistema podem ser excluídas usando este comando. Use **Wbadmin Get Items** para exibir o tipo de versão.|
|-deleteOldest|Exclui o backup de estado do sistema mais antigo.|
|-backupTarget|Especifica o local de armazenamento do backup que você deseja excluir. O local de armazenamento para backups de discos pode ser uma letra de unidade, um ponto de montagem ou um caminho de volume baseado em GUID. Esse valor só precisa ser especificado para localizar backups que não sejam do computador local. As informações sobre backups do computador local estarão disponíveis no catálogo de backup no computador local.|
|-computador|Especifica o computador cujo backup de estado do sistema você deseja excluir. Útil quando é feito o backup de vários computadores no mesmo local. Deve ser usado quando o parâmetro **-backupTarget** é especificado.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="examples"></a>Exemplos

Para excluir o backup de estado do sistema criado em 31 de março de 2013 às 10:00, digite:
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
Para excluir todos os backups de estado do sistema, exceto os três mais recentes, digite:
```
wbadmin delete systemstatebackup -keepVersions:3
```
Para excluir o backup de estado do sistema mais antigo armazenado no disco f:, digite:
```
wbadmin delete systemstatebackup -backupTarget:f:\ -deleteOldest
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [Wbadmin](wbadmin.md)
