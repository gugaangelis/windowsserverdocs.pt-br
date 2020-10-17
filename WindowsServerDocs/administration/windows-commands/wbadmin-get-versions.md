---
title: wbadmin get versions
description: Artigo de referência para o comando Wbadmin Get Versions, que lista detalhes sobre os backups disponíveis que são armazenados no computador local ou em outro computador.
ms.topic: reference
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6825b515f028046702283a2374de26100fd3015f
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155927"
---
# <a name="wbadmin-get-versions"></a>wbadmin get versions

Lista detalhes sobre os backups disponíveis que são armazenados no computador local ou em outro computador. Os detalhes fornecidos para um backup incluem o tempo de backup, o local de armazenamento de backup, o identificador de versão e o tipo de recuperações que você pode executar.

Para obter detalhes sobre os backups disponíveis usando esse comando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

Se esse comando for usado sem parâmetros, ele listará todos os backups do computador local, mesmo que esses backups não estejam disponíveis.

## <a name="syntax"></a>Sintaxe

```
wbadmin get versions [-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}] [-machine:BackupMachineName]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -backupTarget | Especifica o local de armazenamento que contém os backups dos quais você deseja obter os detalhes. Use para listar backups armazenados nesse local de destino. Os locais de destino de backup podem ser unidades de disco conectadas localmente, volumes, pastas compartilhadas remotas, mídia removível, como unidades de DVD ou outras mídias ópticas. Se esse comando for executado no mesmo computador em que o backup foi criado, esse parâmetro não será necessário. No entanto, esse parâmetro é necessário para obter informações sobre um backup criado a partir de outro computador. |
| -computador | Especifica o computador para o qual você deseja obter detalhes de backup. Use quando os backups de vários computadores estiverem armazenados no mesmo local. Deve ser usado quando **-backupTarget** é especificado. |

## <a name="examples"></a>Exemplos

Para ver uma lista de backups disponíveis que são armazenados no volume H:, digite:

```
wbadmin get versions -backupTarget:H:
```

Para ver uma lista de backups disponíveis que são armazenados na pasta compartilhada remota `\\<servername>\<share>` para o computador Server01, digite:

```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin Get Items](wbadmin-get-items.md)

- [Get-WBBackupTarget](/powershell/module/windowserverbackup/Get-WBBackupTarget)
