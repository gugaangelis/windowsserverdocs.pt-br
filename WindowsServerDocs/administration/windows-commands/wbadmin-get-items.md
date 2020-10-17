---
title: wbadmin get items
description: Artigo de referência para o comando Wbadmin Get Items, que lista os itens incluídos em um backup específico.
ms.topic: reference
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5eda5977c0c7606f0e1031894360a06205044656
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155870"
---
# <a name="wbadmin-get-items"></a>wbadmin get items

Lista os itens incluídos em um backup específico.

Para listar os itens incluídos em um backup específico usando esse comando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

## <a name="syntax"></a>Sintaxe

```
wbadmin get items -version:<VersionIdentifier> [-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}] [-machine:<BackupMachineName>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -version | Especifica a versão do backup no formato MM/DD/AAAA-HH: MM. Se você não souber as informações de versão, execute o comando [Wbadmin Get Versions](wbadmin-get-versions.md) . |
| -backupTarget | Especifica o local de armazenamento que contém os backups dos quais você deseja os detalhes. Use para listar backups armazenados nesse local de destino. Os locais de destino do backup podem ser uma unidade de disco conectada localmente ou uma pasta compartilhada remota. Se esse comando for executado no mesmo computador em que o backup foi criado, esse parâmetro não será necessário. No entanto, esse parâmetro é necessário para obter informações sobre um backup criado a partir de outro computador. |
| -computador | Especifica o nome do computador para o qual você deseja obter os detalhes do backup. Útil quando é feito o backup de vários computadores no mesmo local. Deve ser usado quando **-backupTarget** é especificado. |

## <a name="examples"></a>Exemplos

Para listar itens do backup que foi executado em 31 de março de 2013 às 9:00, digite:

```
wbadmin get items -version:03/31/2013-09:00
```

Para listar itens do backup de Server01 que foi executado em 30 de abril de 2013 às 9:00. e armazenados em `\\<servername>\<share>` , digite:

```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin Get Versions](wbadmin-get-versions.md)

- [Get-WBBackupSet](/powershell/module/windowserverbackup/Get-WBBackupSet)
