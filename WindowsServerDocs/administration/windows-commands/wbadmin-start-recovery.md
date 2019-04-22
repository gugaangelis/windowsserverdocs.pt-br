---
title: WBADMIN iniciar recuperação
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f24c9dfeb0ce87474e58d3bd2bce8b68e31cb63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823277"
---
# <a name="wbadmin-start-recovery"></a>WBADMIN iniciar recuperação



Executa uma operação de recuperação com base nos parâmetros que você especificar.

Para executar uma recuperação com este subcomando, você deve ser um membro dos **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um prompt de comando com privilégios elevados, clique em **inicie**, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador**.)

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

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-versão|Especifica o identificador da versão do backup para recuperar no MM/DD/AAAA-formato hh: mm. Se você não souber o identificador de versão, digite **wbadmin obter versões**.|
|-items|Especifica uma lista delimitada por vírgulas de volumes, aplicativos, arquivos ou pastas para recuperar.</br>-Se **- itemtype** é **Volume**, você pode especificar apenas um único volume — fornecendo a letra de unidade do volume, o ponto de montagem de volume ou o nome de volume baseada em GUID.</br>-Se **- itemtype** é **aplicativo**, você pode especificar apenas um único aplicativo. Para ser recuperado, o aplicativo deve ter registrado com o Backup do Windows Server. Você também pode usar o valor **ADIFM** para recuperar uma instalação do Active Directory. Consulte os comentários para obter mais informações.</br>-Se **- itemtype** é **arquivo**, você pode especificar arquivos ou pastas, mas eles devem fazer parte do mesmo volume e eles devem estar sob a mesma pasta pai.|
|-itemtype|Especifica o tipo de itens que serão recuperados. Deve ser **Volume**, **App**, ou **arquivo**.|
|-backupTarget|Especifica o local de armazenamento que contém o backup que você deseja recuperar. Esse parâmetro é útil quando o local é diferente de onde os backups desse computador geralmente são armazenados.|
|-machine|Especifica o nome do computador que você deseja recuperar o backup. Esse parâmetro é útil quando vários computadores tiverem sido copiados para o mesmo local. Ele deve ser usado quando o **- backupTarget** parâmetro for especificado.|
|-recoveryTarget|Especifica o local para restaurar para. Esse parâmetro é útil se esse local é diferente do local que foi feito backup anteriormente. Ele também pode ser usado para restaurações de volumes, arquivos ou aplicativos. Se você estiver restaurando um volume, você pode especificar a letra de unidade do volume do volume alternativo. Se você estiver restaurando um arquivo ou aplicativo, você pode especificar um local de recuperação alternativo.|
|-recursivo|Válido somente quando a recuperação de arquivos. Recupera os arquivos nas pastas e todos os arquivos subordinados para as pastas especificadas. Por padrão, somente os arquivos que residem diretamente nas pastas especificadas são recuperados.|
|-overwrite|Válido somente quando a recuperação de arquivos. Especifica a ação a ser tomada quando um arquivo que está sendo recuperado já existir no mesmo local.</br>-   **Ignorar** faz com que o Backup do Windows Server ignorar o arquivo existente e continuar com a recuperação do próximo arquivo.</br>-   **CreateCopy** faz com que o Backup do Windows Server criar uma cópia do arquivo existente para que o arquivo existente não será modificado.</br>-   **Substituir** faz com que o Backup do Windows Server para substituir o arquivo existente com o arquivo do backup.|
|-notRestoreAcl|Válido somente quando a recuperação de arquivos. Especifica as listas de controle de acesso (ACLs) segurança dos arquivos sendo recuperados a partir do backup para restauração não. Por padrão, as ACLs de segurança são restauradas (o valor padrão é **verdadeira)**. Se esse parâmetro for usado, as ACLs para os arquivos restaurados serão herdadas do local para o qual os arquivos estão sendo restaurados.|
|-skipBadClusterCheck|Válido somente quando a recuperação de volumes. Ignora a verificação de discos que você está fazendo a recuperação para obter informações de cluster incorreta. Se você estiver recuperando para um servidor alternativo ou hardware, é recomendável que você não use esse parâmetro. Você pode executar manualmente o comando **/chkdsk b** nesses discos a qualquer momento para verificá-los para clusters inválidos e, em seguida, atualize as informações do sistema de arquivos adequadamente.</br>Importante: Até que você execute **chkdsk** conforme descrito, os clusters inválidos relatados em seu sistema recuperado podem não ser precisos.|
|-noRollForward|Válido somente quando a recuperação de aplicativos. Permite recuperação point-in-time anterior de um aplicativo se a versão mais recente de backups é selecionada. Para outras versões do aplicativo que não são a recuperação point-in-time anterior, mais recente é feita como o padrão.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="remarks"></a>Comentários

-   Para exibir uma lista de itens que estão disponíveis para recuperação de uma versão de backup específica, use **wbadmin obter itens**. Se um volume não tem uma letra de unidade ou ponto de montagem no momento do backup, este subcomando retornaria um nome de volume baseada em GUID deve ser usado para recuperar o volume.
-   Quando o **- itemtype** é **App**, você pode usar um valor de **ADIFM** para **-item** para executar uma instalação da operação de mídia para recuperar todos os o dados relacionados necessários para os serviços de domínio do Active Directory. **ADIFM** cria uma cópia do banco de dados do Active Directory, do registro e do estado SYSVOL e, em seguida, salva essas informações no local especificado por **- recoveryTarget**. Use esse parâmetro somente quando **recoveryTarget -** for especificado.

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="BKMK_Examples"></a>Exemplos

Para executar uma recuperação do backup 31 de março de 2013, feitos às 9h00, de d: de volume, digite:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
Para executar uma recuperação de unidade d do backup 31 de março de 2013, feitos às 9h00, do registro, digite:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
Para executar uma recuperação do backup 31 de março de de 2013, feitos às 9h00, d:\folder e pastas subordinadas a d:\folder, digite:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
Para executar uma recuperação do backup de 31 de março de 2013, feitos às 9h00, do volume \\ \\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\, tipo:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Para executar uma recuperação do backup de 30 de abril de 2013, feitos às 9h00, da pasta compartilhada \\ \\servername\share de server01, digite:
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx) cmdlet
-   [Iniciar WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx) cmdlet
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet
-   [Iniciar WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx) cmdlet