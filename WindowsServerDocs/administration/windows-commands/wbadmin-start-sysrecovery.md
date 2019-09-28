---
title: Wbadmin start sysrecovery
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c653fa52a2a56267d6f0df169f8f9924f2aa94d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362283"
---
# <a name="wbadmin-start-sysrecovery"></a>Wbadmin start sysrecovery



Executa uma recuperação do sistema (recuperação bare-metal) usando os parâmetros que você especificar.

> [!NOTE]
> Esse subcomando pode ser executado somente do ambiente de recuperação do Windows e não está listado por padrão no texto de uso de **Wbadmin**. Para obter mais informações, consulte [visão geral do ambiente de recuperação do Windows (Windows re)](https://technet.microsoft.com/library/hh825173.aspx).

Para executar uma recuperação do sistema com esse subcomando, você deve ser um membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas.

Para obter exemplos de como usar esse subcomando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
wbadmin start sysrecovery
-version:<VersionIdentifier>
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-restoreAllVolumes]
[-recreateDisks]
[-excludeDisks]
[-skipBadClusterCheck]
[-quiet]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-versão|Especifica o identificador de versão para o backup a ser recuperado no formato MM/DD/AAAA-HH: MM. Se você não souber o identificador de versão, digite **Wbadmin obter versões**.|
|-backupTarget|Especifica o local de armazenamento que contém os backups ou backups que você deseja recuperar. Esse parâmetro é útil quando o local de armazenamento é diferente de onde os backups deste computador geralmente são armazenados.|
|-computador|Especifica o nome do computador que você deseja recuperar. Esse parâmetro é útil quando é feito o backup de vários computadores no mesmo local. Deve ser usado quando o parâmetro **-backupTarget** é especificado.|
|-restoreAllVolumes|Recupera todos os volumes do backup selecionado. Se esse parâmetro não for especificado, somente os volumes críticos (volumes que contêm o estado do sistema e os componentes do sistema operacional) serão recuperados. Esse parâmetro é útil quando você precisa recuperar volumes não críticos durante a recuperação do sistema.|
|-recreateDisks|Recupera uma configuração de disco para o estado que existia quando o backup foi criado.</br>Aviso: Esse parâmetro exclui todos os dados em volumes que hospedam componentes do sistema operacional. Ele também pode excluir dados de volumes de dados.|
|-excludeDisks|Válido somente quando especificado com o parâmetro **-recreateDisks** e deve ser inserido como uma lista delimitada por vírgulas de identificadores de disco (conforme listado na saída de **Wbadmin obter discos**). Discos excluídos não são particionados ou formatados. Esse parâmetro ajuda a preservar dados em discos que você não deseja modificar durante a operação de recuperação.|
|-skipBadClusterCheck|Ignora a verificação dos discos de recuperação em busca de informações de cluster inválidos. Se você estiver restaurando para um servidor ou hardware alternativo, recomendamos que você não use esse parâmetro. Você pode executar manualmente o **chkdsk/b** em seus discos de recuperação a qualquer momento para verificá-los quanto a clusters inválidos e, em seguida, atualizar as informações do sistema de arquivos de acordo.</br>Aviso: Até que você execute **chkdsk** conforme descrito, os clusters inválidos relatados em seu sistema recuperado podem não ser precisos.|
|-quiet|Executa o comando sem prompts para o usuário.|

## <a name="BKMK_examples"></a>Disso

Para iniciar a recuperação das informações do backup que foi executado em 31 de março de 2013 às 9:00, localizada na unidade d:, digite:
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
Para começar a recuperar as informações do backup que foi executado em 30 de abril de 2013 às 9:00, localizadas na pasta compartilhada \\ @ no__t-1servername\shared: para Server01, digite:
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBBareMetalRecovery](https://technet.microsoft.com/library/jj902461.aspx)