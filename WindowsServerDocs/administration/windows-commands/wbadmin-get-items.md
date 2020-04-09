---
title: Wbadmin obter itens
description: O tópico de comandos do Windows para WBADMIN Get Items, que lista os itens incluídos em um backup específico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 781057bb450ab4b692c793f67d807e008efcf5d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829749"
---
# <a name="wbadmin-get-items"></a>Wbadmin obter itens



Lista os itens incluídos em um backup específico.

Para usar esse subcomando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** , ou você deve ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando** e clique em **Executar como administrador**.)

Para obter exemplos de como usar esse subcomando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-versão|Especifica a versão do backup no formato MM/DD/AAAA-HH: MM. Se você não souber as informações de versão, digite **Wbadmin Get Versions**.|
|-backupTarget|Especifica o local de armazenamento que contém os backups dos quais você deseja os detalhes. Use para listar backups armazenados nesse local de destino. Os locais de destino do backup podem ser uma unidade de disco conectada localmente ou uma pasta compartilhada remota. Se o **Wbadmin Get Items**for executado no mesmo computador em que o backup foi criado, esse parâmetro não será necessário. No entanto, esse parâmetro é necessário para obter informações sobre um backup criado a partir de outro computador.|
|-computador|Especifica o nome do computador para o qual você deseja obter os detalhes do backup. Útil quando é feito o backup de vários computadores no mesmo local. Deve ser usado quando **-backupTarget** é especificado.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para listar itens do backup que foi executado em 31 de março de 2013 às 9:00, digite:
```
wbadmin get items -version:03/31/2013-09:00
```
Para listar itens do backup de Server01 que foi executado em 30 de abril de 2013 às 9:00. e armazenados em \\\\servername\share, digite:
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx)