---
title: wbadmin start systemstaterecovery
description: Artigo de referência para Wbadmin start systemstaterecovery, que executa uma recuperação de estado do sistema em um local e de um backup que você especificar.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c433871cb99018d10d064aac2f7a2098e0d42692
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954428"
---
# <a name="wbadmin-start-systemstaterecovery"></a>wbadmin start systemstaterecovery



Executa uma recuperação de estado do sistema em um local e de um backup que você especificar.

> [!NOTE]
> Backup do Windows Server não faz backup ou recupera hives de usuário do registro (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou da recuperação do estado do sistema.

Para executar uma recuperação de estado do sistema com esse subcomando, você deve ser um membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)



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

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-version|Especifica o identificador de versão para o backup a ser recuperado no formato MM/DD/AAAA-HH: MM. Se você não souber o identificador de versão, digite **Wbadmin obter versões**.|
|-Resumo dos|Relata o resumo da última recuperação de estado do sistema (após a reinicialização necessária para concluir a operação). Este parâmetro não pode ser acompanhado por nenhum outro parâmetro.|
|-backupTarget|Especifica o local de armazenamento que contém os backups ou backups que você deseja recuperar. Esse parâmetro é útil quando o local de armazenamento é diferente de onde os backups desse computador geralmente são armazenados.|
|-computador|Especifica o nome do computador que você deseja recuperar. Esse parâmetro é útil quando é feito o backup de vários computadores no mesmo local. Deve ser usado quando o parâmetro **-backupTarget** é especificado.|
|-recoveryTarget|Especifica o diretório para o qual restaurar. Esse parâmetro será útil se o backup for restaurado para um local alternativo.|
|-authsysvol|Se usado, executa uma restauração autoritativa do SYSVOL (o diretório de volume compartilhado do sistema).|
|-reinicialização|Especifica a reinicialização do sistema no final da operação de recuperação do estado do sistema. Esse parâmetro é válido somente para uma recuperação no local original. Não recomendamos que você use esse parâmetro se precisar executar etapas após a operação de recuperação.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="examples"></a>Exemplos

- Para executar uma recuperação de estado do sistema do backup de 03/31/2013 às 9:00, digite:
  ```
  wbadmin start systemstaterecovery -version:03/31/2013-09:00
  ```
- Para executar uma recuperação de estado do sistema do backup de 04/30/2013 às 9:00. que está armazenado no recurso compartilhado \\ \\ servername\share para Server01, digite:
  ```
  wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
  ```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Start-WBSystemStateRecovery](/previous-versions/windows/it-pro/windows-8.1-and-8/hh825173(v=win.10))
