---
title: Wbadmin iniciar recuperação
description: O tópico de comandos do Windows para Wbadmin start Recovery, que executa uma operação de recuperação com base nos parâmetros que você especificar.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b5a65e67e7a34ca5263c85c1038820e0a4fc1ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829609"
---
# <a name="wbadmin-start-recovery"></a>Wbadmin iniciar recuperação



Executa uma operação de recuperação com base nos parâmetros que você especificar.

Para executar uma recuperação com este subcomando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique em **Iniciar**, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

Para obter exemplos de como usar esse subcomando, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
wbadmin start recovery
-version:<VersionIdentifier>
-items:{<VolumesToRecover> | <AppsToRecover> | <FilesOrFoldersToRecover>}
-itemtype:{Volume | App | File}
[-backupTarget:{<VolumeHostingBackup> | <NetworkShareHostingBackup>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:{<TargetVolumeForRecovery> | <TargetPathForRecovery>}]
[-recursive]
[-overwrite:{Overwrite | CreateCopy | Skip}]
[-notRestoreAcl]
[-skipBadClusterCheck]
[-noRollForward]
[-quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-versão|Especifica o identificador de versão do backup a ser recuperado no formato MM/DD/AAAA-HH: MM. Se você não souber o identificador de versão, digite **Wbadmin obter versões**.|
|-itens|Especifica uma lista delimitada por vírgulas de volumes, aplicativos, arquivos ou pastas a serem recuperados.</br>-Se **-ItemType** for **volume**, você poderá especificar apenas um único volume — fornecendo a letra da unidade do volume, o ponto de montagem do volume ou o nome do volume baseado em GUID.</br>-Se **-ItemType** for um **aplicativo**, você poderá especificar apenas um único aplicativo. Para ser recuperado, o aplicativo deve ter sido registrado com Backup do Windows Server. Você também pode usar o valor **ADIFM** para recuperar uma instalação do Active Directory. Consulte comentários em para obter mais informações.</br>-Se **-ItemType** for um **arquivo**, você pode especificar arquivos ou pastas, mas eles devem fazer parte do mesmo volume e devem estar na mesma pasta pai.|
|-ItemType|Especifica o tipo de itens a serem recuperados. Deve ser **volume**, **aplicativo**ou **arquivo**.|
|-backupTarget|Especifica o local de armazenamento que contém o backup que você deseja recuperar. Esse parâmetro é útil quando o local é diferente de onde os backups deste computador geralmente são armazenados.|
|-computador|Especifica o nome do computador para o qual você deseja recuperar o backup. Esse parâmetro é útil quando é feito o backup de vários computadores no mesmo local. Ele deve ser usado quando o parâmetro **-backupTarget** é especificado.|
|-recoveryTarget|Especifica o local para o qual restaurar. Esse parâmetro será útil se esse local for diferente do local em que o backup foi feito anteriormente. Ele também pode ser usado para restaurações de volumes, arquivos ou aplicativos. Se você estiver restaurando um volume, poderá especificar a letra da unidade de volume do volume alternativo. Se você estiver restaurando um arquivo ou aplicativo, poderá especificar um local de recuperação alternativo.|
|-recursivo|Válido somente ao recuperar arquivos. Recupera os arquivos nas pastas e todos os arquivos subordinados às pastas especificadas. Por padrão, somente os arquivos que residem diretamente nas pastas especificadas são recuperados.|
|-substituir|Válido somente ao recuperar arquivos. Especifica a ação a ser tomada quando um arquivo que está sendo recuperado já existir no mesmo local.</br>-   **ignorar** faz com que backup do Windows Server ignore o arquivo existente e continue com a recuperação do próximo arquivo.</br>-   **CreateCopy** faz com que backup do Windows Server crie uma cópia do arquivo existente para que o arquivo existente não seja modificado.</br>-   **substituir** faz backup do Windows Server substituir o arquivo existente pelo arquivo do backup.|
|-notRestoreAcl|Válido somente ao recuperar arquivos. Especifica para não restaurar as listas de controle de acesso (ACLs) de segurança dos arquivos que estão sendo recuperados do backup. Por padrão, as ACLs de segurança são restauradas (o valor padrão é **true)** . Se esse parâmetro for usado, as ACLs para os arquivos restaurados serão herdadas do local para o qual os arquivos estão sendo restaurados.|
|-skipBadClusterCheck|Válido somente ao recuperar volumes. Ignora a verificação dos discos que você está recuperando para informações de cluster inválidos. Se você estiver recuperando para um servidor ou hardware alternativo, recomendamos que você não use esse parâmetro. Você pode executar manualmente o comando **chkdsk/b** nesses discos a qualquer momento para verificá-los em busca de clusters inválidos e, em seguida, atualizar as informações do sistema de arquivos de acordo.</br>Importante: até que você execute **chkdsk** conforme descrito, os clusters inválidos relatados em seu sistema recuperado podem não ser precisos.|
|-noRollForward|Válido somente ao recuperar aplicativos. Permite a recuperação pontual anterior de um aplicativo se a versão mais recente dos backups estiver selecionada. Para outras versões do aplicativo que não são as mais recentes, a recuperação pontual anterior é feita como o padrão.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="remarks"></a>Comentários

-   Para exibir uma lista de itens que estão disponíveis para recuperação de uma versão de backup específica, use **Wbadmin Get Items**. Se um volume não tiver um ponto de montagem ou uma letra da unidade no momento do backup, esse subcomando retornará um nome de volume baseado em GUID que deveria ser usado para recuperar o volume.
-   Quando o **-ItemType** é um **aplicativo**, você pode usar um valor de **ADIFM** para **-Item** para executar uma operação de instalação de mídia para recuperar todos os dados relacionados necessários para Active Directory Domain Services. O **ADIFM** cria uma cópia do banco de dados do Active Directory, do registro e do estado do SYSVOL e, em seguida, salva essas informações no local especificado por **-recoveryTarget**. Use este parâmetro somente quando **-recoveryTarget** for especificado.

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Para executar uma recuperação do backup de 31 de março de 2013, obtido às 9:00 A.M., do volume d:, digite:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
Para executar uma recuperação na unidade d do backup de 31 de março de 2013, obtido às 9:00 A.M., do registro, digite:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
Para executar uma recuperação do backup de 31 de março de 2013, obtido às 9:00 A.M., do d:\folder e das pastas subordinadas a d:\folder, digite:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
Para executar uma recuperação do backup de 31 de março de 2013, obtido às 9:00 A.M., do volume \\\\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\, tipo:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Para executar uma recuperação do backup de 30 de abril de 2013, obtido às 9:00 A.M., da pasta compartilhada \\\\servername\share de Server01, digite:
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Start-WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx)
-   Cmdlet [Start-WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx)
-   Cmdlet [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx)
-   Cmdlet [Start-WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx)