---
title: wbadmin start recovery
description: Artigo de referência para o comando Wbadmin start Recovery, que executa uma operação de recuperação com base nos parâmetros que você especificar.
ms.topic: reference
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 408ccadd830e50e9b51447cc19f7243d349cb3ef
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524751"
---
# <a name="wbadmin-start-recovery"></a>wbadmin start recovery

Executa uma operação de recuperação com base nos parâmetros que você especificar.

Para executar uma recuperação usando esse comando, você deve ser um membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

## <a name="syntax"></a>Sintaxe

```
wbadmin start recovery -version:<VersionIdentifier> -items:{<VolumesToRecover> | <AppsToRecover> | <FilesOrFoldersToRecover>} -itemtype:{Volume | App | File} [-backupTarget:{<VolumeHostingBackup> | <NetworkShareHostingBackup>}] [-machine:<BackupMachineName>] [-recoveryTarget:{<TargetVolumeForRecovery> | <TargetPathForRecovery>}] [-recursive] [-overwrite:{Overwrite | CreateCopy | Skip}] [-notRestoreAcl] [-skipBadClusterCheck] [-noRollForward] [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -version | Especifica o identificador de versão do backup a ser recuperado no formato MM/DD/AAAA-HH: MM. Se você não souber o identificador de versão, execute o [comando Wbadmin Get Versions](wbadmin-get-versions.md). |
| -itens | Especifica uma lista delimitada por vírgulas de volumes, aplicativos, arquivos ou pastas a serem recuperados. Você deve usar esse parâmetro com o parâmetro **-ItemType** . |
| -ItemType | Especifica o tipo de itens a serem recuperados. Deve ser **volume**, **aplicativo**ou **arquivo**. Se o **-ItemType** for *volume*, você poderá especificar apenas um único volume, fornecendo a letra da unidade do volume, o ponto de montagem do volume ou o nome do volume baseado em GUID. Se o **-ItemType** for um *aplicativo*, você poderá especificar apenas um único aplicativo ou usar o valor **ADIFM** para recuperar uma instalação do Active Directory. Para ser recuperado, o aplicativo deve ter sido registrado com Backup do Windows Server. Se o **-ItemType** for um *arquivo*, você poderá especificar arquivos ou pastas, mas eles deverão fazer parte do mesmo volume e deverão estar na mesma pasta pai. |
| -backupTarget | Especifica o local de armazenamento que contém o backup que você deseja recuperar. Esse parâmetro é útil quando o local é diferente de onde os backups deste computador geralmente são armazenados. |
| -computador | Especifica o nome do computador para o qual você deseja recuperar o backup. Esse parâmetro deve ser usado quando o parâmetro **-backupTarget** é especificado. O parâmetro **-Machine** é útil quando é feito o backup de vários computadores no mesmo local. |
| -recoveryTarget | Especifica o local para o qual restaurar. Esse parâmetro será útil se esse local for diferente do local em que o backup foi feito anteriormente. Ele também pode ser usado para restaurações de volumes, arquivos ou aplicativos. Se estiver restaurando um volume, você poderá especificar a letra da unidade de volume do volume alternativo. Se você estiver restaurando um arquivo ou aplicativo, poderá especificar um local de recuperação alternativo. |
| -recursivo | Válido somente ao recuperar arquivos. Recupera os arquivos nas pastas e todos os arquivos subordinados às pastas especificadas. Por padrão, somente os arquivos que residem diretamente nas pastas especificadas são recuperados. |
|-substituir | Válido somente ao recuperar arquivos. Especifica a ação a ser tomada quando um arquivo que está sendo recuperado já existir no mesmo local. As opções válidas são:<ul><li>**Skip** -faz com que backup do Windows Server ignore o arquivo existente e continue com a recuperação do próximo arquivo.</li><li>**CreateCopy** -faz com que backup do Windows Server crie uma cópia do arquivo existente para que o arquivo existente não seja modificado.</li><li>**Substituir** -faz com que backup do Windows Server substitua o arquivo existente pelo arquivo do backup. |
| -notRestoreAcl | Válido somente ao recuperar arquivos. Especifica para não restaurar as listas de controle de acesso (ACLs) de segurança dos arquivos que estão sendo recuperados do backup. Por padrão, as ACLs de segurança são restauradas (o valor padrão é **true**). Se esse parâmetro for usado, as ACLs para os arquivos restaurados serão herdadas do local para o qual os arquivos estão sendo restaurados. |
| -skipBadClusterCheck | Válido somente ao recuperar volumes. Ignora a verificação dos discos que você está recuperando para informações de cluster inválidos. Se você estiver recuperando para um servidor ou hardware alternativo, recomendamos que você não use esse parâmetro. Você pode executar manualmente o comando **chkdsk/b** nesses discos a qualquer momento para verificá-los em busca de clusters inválidos e, em seguida, atualizar as informações do sistema de arquivos de acordo.<p>**Importante:** Até que você execute **chkdsk/b**, os clusters inválidos relatados em seu sistema recuperado podem não ser precisos. |
| -noRollForward | Válido somente ao recuperar aplicativos. Permite a recuperação pontual anterior de um aplicativo se você selecionar a versão mais recente dos backups. A recuperação pontual anterior é feita como o padrão para todas as outras versões não mais recentes do aplicativo. |
| -quiet | Executa o comando sem prompts para o usuário. |

#### <a name="remarks"></a>Comentários

- Para exibir uma lista de itens disponíveis para recuperação de uma versão de backup específica, execute o [comando Wbadmin Get Items](wbadmin-get-items.md). Se um volume não tiver um ponto de montagem ou uma letra da unidade no momento do backup, esse comando retornará um nome de volume baseado em GUID que deve ser usado para recuperar o volume.

- Se você usar um valor de **ADIFM** para executar uma operação de instalação de mídia a fim de recuperar os dados relacionados necessários para Active Directory Domain Services, o **ADIFM** criará uma cópia do estado de Active Directory, do registro e do SYSVOL e, em seguida, salvará essas informações no local especificado por **-recoveryTarget**. Use este parâmetro somente quando **-recoveryTarget** for especificado.

## <a name="examples"></a>Exemplos

Para executar uma recuperação do backup de 31 de março de 2020, obtido às 9:00 A.M., do volume d:, digite:

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:Volume -items:d:
```

Para executar uma recuperação na unidade d do backup de 31 de março de 2020, obtido às 9:00 A.M., do registro, digite:

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```

Para executar uma recuperação do backup de 31 de março de 2020, obtido às 9:00 A.M., do d:\folder e das pastas subordinadas a d:\folder, digite:

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:File -items:d:\folder -recursive
```

Para executar uma recuperação do backup de 31 de março de 2020, obtido às 9:00 A.M., do volume `\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\` , digite:

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:Volume -items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```

Para executar uma recuperação do backup de 30 de abril de 2020, obtido às 9:00 A.M., da pasta compartilhada `\\servername\share` de Server01, digite:

```
wbadmin start recovery -version:04/30/2020-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [Start-WBFileRecovery](/powershell/module/windowserverbackup/Start-WBFileRecovery)

- [Start-WBHyperVRecovery](/powershell/module/windowserverbackup/Start-WBHyperVRecovery)

- [Start-WBSystemStateRecovery](/powershell/module/windowserverbackup/Start-WBSystemStateRecovery)

- [Start-WBVolumeRecovery](/powershell/module/windowserverbackup/Start-WBVolumeRecovery)
