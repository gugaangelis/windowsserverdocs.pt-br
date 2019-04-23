---
title: Wbadmin start sysrecovery
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e8e0ff114d09d70b9e50e8c4ea6af6330c74128c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847157"
---
# <a name="wbadmin-start-sysrecovery"></a>Wbadmin start sysrecovery



Executa uma recuperação do sistema (recuperação bare-metal) usando os parâmetros que você especificar.

> [!NOTE]
> Esse subcomando pode ser executado somente a partir do ambiente de recuperação do Windows, e não estiver listado por padrão no texto do uso **Wbadmin**. Para obter mais informações, consulte [visão geral do ambiente de recuperação do Windows (Windows RE)](https://technet.microsoft.com/library/hh825173.aspx).

Para executar uma recuperação do sistema com essa subcomando, você deve ser um membro dos **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas.

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
|-versão|Especifica o identificador de versão para o backup recuperar no MM/DD/AAAA-formato hh: mm. Se você não souber o identificador de versão, digite **wbadmin obter versões**.|
|-backupTarget|Especifica o local de armazenamento que contém o backup ou backups que você deseja recuperar. Esse parâmetro é útil quando o local de armazenamento é diferente de onde os backups desse computador geralmente são armazenados.|
|-machine|Especifica o nome do computador que você deseja recuperar. Esse parâmetro é útil quando vários computadores tiverem sido copiados para o mesmo local. Deve ser usado quando o **- backupTarget** parâmetro for especificado.|
|-restoreAllVolumes|Recupera todos os volumes de backup selecionado. Se esse parâmetro não for especificado, os volumes só críticos (volumes que contêm os componentes de sistema operacional e estado do sistema) são recuperados. Esse parâmetro é útil quando você precisar recuperar volumes não críticos durante a recuperação do sistema.|
|-recreateDisks|Recupera uma configuração de disco para o estado que existia quando o backup foi criado.</br>Aviso: Este parâmetro exclui todos os dados em volumes pelos componentes de sistema operacional do host. Ele também pode excluir os dados dos volumes de dados.|
|-excludeDisks|Válido somente quando especificado com o **- recreateDisks** parâmetro e deverão ser inseridos como uma lista delimitada por vírgulas de identificadores de disco (conforme listado na saída do **wbadmin obter discos**). Discos excluídos não são particionados ou formatados. Esse parâmetro ajuda a preservar os dados nos discos que você deseja não modificados durante a operação de recuperação.|
|-skipBadClusterCheck|Ignora a verificação de seus discos de recuperação para obter informações de cluster incorreta. Se você estiver restaurando para um servidor alternativo ou hardware, é recomendável que você não use esse parâmetro. Você pode executar manualmente **/chkdsk b** em seus discos de recuperação a qualquer momento para verificá-los para clusters inválidos e, em seguida, atualize as informações do sistema de arquivos adequadamente.</br>Aviso: Até que você execute **chkdsk** conforme descrito, os clusters inválidos relatados em seu sistema recuperado podem não ser precisos.|
|-quiet|Executa o comando sem prompts para o usuário.|

## <a name="BKMK_examples"></a>Exemplos

Para começar a recuperar as informações de backup que foi executado em 31 de março de 2013 às 9h00, localizado na unidade d:, digite:
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
Para começar a recuperar as informações de backup que foi executado em 30 de abril de 2013 às 9h00, localizado na pasta compartilhada \\ \\servername\shared: para o server01, digite:
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBareMetalRecovery](https://technet.microsoft.com/library/jj902461.aspx) cmdlet