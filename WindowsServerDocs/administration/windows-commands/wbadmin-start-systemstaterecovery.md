---
title: wbadmin start systemstaterecovery
description: Artigo de referência para o comando Wbadmin start systemstaterecovery, que executa uma recuperação de estado do sistema em um local e de um backup que você especificar.
ms.topic: reference
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b52b4fedf14eb2d4caab4e9d521767ff9e89365a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524711"
---
# <a name="wbadmin-start-systemstaterecovery"></a>wbadmin start systemstaterecovery

Executa uma recuperação de estado do sistema em um local e de um backup que você especificar.

Para executar uma recuperação de estado do sistema usando esse comando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

> [!NOTE]
> Backup do Windows Server não faz backup ou recupera hives de usuário do registro (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou da recuperação do estado do sistema.

## <a name="syntax"></a>Sintaxe

```
wbadmin start systemstaterecovery -version:<VersionIdentifier> -showsummary [-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>] [-recoveryTarget:<TargetPathForRecovery>] [-authsysvol] [-autoReboot] [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -version | Especifica o identificador de versão do backup a ser recuperado no formato MM/DD/AAAA-HH: MM. Se você não souber o identificador de versão, execute o [comando Wbadmin Get Versions](wbadmin-get-versions.md). |
| -Resumo dos | Relata o resumo da última recuperação de estado do sistema (após a reinicialização necessária para concluir a operação). Esse parâmetro não pode ser acompanhado por nenhum outro parâmetro. |
| -backupTarget | Especifica o local de armazenamento com os backups que você deseja recuperar. Esse parâmetro é útil quando o local de armazenamento é diferente de onde os backups são normalmente armazenados. |
| -computador | Especifica o nome do computador para o qual recuperar o backup. Esse parâmetro deve ser usado quando o parâmetro **-backupTarget** é especificado. O parâmetro **-Machine** é útil quando é feito o backup de vários computadores no mesmo local. |
| -recoveryTarget | Especifica em que diretório restaurar. Esse parâmetro será útil se o backup for restaurado para um local alternativo. |
| -authsysvol | Executa uma restauração autoritativa do diretório compartilhado do volume do sistema (SYSVOL). |
| -reinicialização | Especifica a reinicialização do sistema no final da operação de recuperação do estado do sistema. Esse parâmetro é válido somente para uma recuperação no local original. Não recomendamos que você use esse parâmetro se precisar executar etapas após a operação de recuperação. |
| -quiet | Executa o comando sem prompts para o usuário. |

## <a name="examples"></a>Exemplos

Para iniciar uma recuperação de estado do sistema do backup de 03/31/2020 às 9:00, digite:

```
wbadmin start systemstaterecovery -version:03/31/2020-09:00
```

Para iniciar uma recuperação de estado do sistema do backup de 04/30/2020 às 9:00 A.M. que é armazenado no recurso compartilhado `\\servername\share` para Server01, digite:

```
wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [Start-WBSystemStateRecovery](/powershell/module/windowserverbackup/Start-WBSystemStateRecovery)
