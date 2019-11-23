---
title: Wbadmin start systemstaterecovery
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ae534eed26629be264b698869edc57232e2b571
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362216"
---
# <a name="wbadmin-start-systemstaterecovery"></a>Wbadmin start systemstaterecovery



Executa uma recuperação de estado do sistema em um local e de um backup que você especificar.

> [!NOTE]
> Backup do Windows Server não faz backup ou recupera hives de usuário do registro (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou da recuperação do estado do sistema.

Para executar uma recuperação de estado do sistema com esse subcomando, você deve ser um membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

Sintaxe do Windows Server 2008:
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-quiet]
```
Sintaxe do Windows Server 2008 R2 ou posterior:
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-autoReboot]
[-quiet]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-versão|Especifica o identificador de versão para o backup a ser recuperado no formato MM/DD/AAAA-HH: MM. Se você não souber o identificador de versão, digite **Wbadmin obter versões**.|
|-Resumo dos|Relata o resumo da última recuperação de estado do sistema (após a reinicialização necessária para concluir a operação). Este parâmetro não pode ser acompanhado por nenhum outro parâmetro.|
|-backupTarget|Especifica o local de armazenamento que contém os backups ou backups que você deseja recuperar. Esse parâmetro é útil quando o local de armazenamento é diferente de onde os backups desse computador geralmente são armazenados.|
|-computador|Especifica o nome do computador que você deseja recuperar. Esse parâmetro é útil quando é feito o backup de vários computadores no mesmo local. Deve ser usado quando o parâmetro **-backupTarget** é especificado.|
|-recoveryTarget|Especifica o diretório para o qual restaurar. Esse parâmetro será útil se o backup for restaurado para um local alternativo.|
|-authsysvol|Se usado, executa uma restauração autoritativa do SYSVOL (o diretório de volume compartilhado do sistema).|
|-reinicialização|Especifica a reinicialização do sistema no final da operação de recuperação do estado do sistema. Esse parâmetro é válido somente para uma recuperação no local original. Não recomendamos que você use esse parâmetro se precisar executar etapas após a operação de recuperação.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="BKMK_examples"></a>Disso

- Para executar uma recuperação de estado do sistema do backup de 03/31/2013 às 9:00, digite:  
  ```
  wbadmin start systemstaterecovery -version:03/31/2013-09:00
  ```  
- Para executar uma recuperação de estado do sistema do backup de 04/30/2013 às 9:00. que está armazenado no recurso compartilhado \\\\servername\share para Server01, digite:  
  ```
  wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
  ```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx)