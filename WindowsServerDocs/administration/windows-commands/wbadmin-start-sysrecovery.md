---
title: wbadmin start sysrecovery
description: Artigo de referência para o comando Wbadmin start sysrecovery, que executa uma recuperação do sistema (recuperação bare-metal) usando os parâmetros especificados.
ms.topic: reference
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 292c0d6fc42f4fe58dda6a6fbbb6a693b70a8756
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524741"
---
# <a name="wbadmin-start-sysrecovery"></a>wbadmin start sysrecovery

Executa uma recuperação do sistema (recuperação bare-metal) usando os parâmetros especificados.

Para executar uma recuperação do sistema usando esse comando, você deve ser um membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas.

> [!IMPORTANT]
> O comando **Wbadmin start sysrecovery** deve ser executado no console de recuperação do Windows e não está listado no texto de uso padrão da ferramenta **Wbadmin** . Para obter mais informações, consulte [ambiente de recuperação do Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxe

```
wbadmin start sysrecovery -version:<VersionIdentifier> -backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}  [-machine:<BackupMachineName>] [-restoreAllVolumes] [-recreateDisks] [-excludeDisks] [-skipBadClusterCheck] [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -version | Especifica o identificador de versão do backup a ser recuperado no formato MM/DD/AAAA-HH: MM. Se você não souber o identificador de versão, execute o [comando Wbadmin Get Versions](wbadmin-get-versions.md). |
| -backupTarget | Especifica o local de armazenamento que contém os backups que você deseja recuperar. Esse parâmetro é útil quando o local de armazenamento é diferente de onde os backups deste computador geralmente são armazenados. |
| -computador | Especifica o nome do computador para o qual você deseja recuperar o backup. Esse parâmetro deve ser usado quando o parâmetro **-backupTarget** é especificado. O parâmetro **-Machine** é útil quando é feito o backup de vários computadores no mesmo local. |
| -restoreAllVolumes | Recupera todos os volumes do backup selecionado. Se esse parâmetro não for especificado, somente os volumes críticos (volumes que contêm o estado do sistema e os componentes do sistema operacional) serão recuperados. Esse parâmetro é útil quando você precisa recuperar volumes não críticos durante a recuperação do sistema. |
| -recreateDisks | Recupera uma configuração de disco para o estado que existia quando o backup foi criado.<p>**AVISO:** Esse parâmetro exclui todos os dados em volumes que hospedam componentes do sistema operacional. Ele também pode excluir dados de volumes de dados. |
| -excludeDisks | Válido somente quando especificado com o parâmetro **-recreateDisks** e deve ser inserido como uma lista delimitada por vírgulas de identificadores de disco (conforme listado na saída do [comando Wbadmin Get discos](wbadmin-get-disks.md)). Os discos excluídos não são particionados nem formatados. Esse parâmetro ajuda a preservar dados em discos que você não deseja modificar durante a operação de recuperação. |
| -skipBadClusterCheck | Válido somente ao recuperar volumes. Ignora a verificação dos discos que você está recuperando para informações de cluster inválidos. Se você estiver recuperando para um servidor ou hardware alternativo, recomendamos que você não use esse parâmetro. Você pode executar manualmente o comando **chkdsk/b** nesses discos a qualquer momento para verificá-los em busca de clusters inválidos e, em seguida, atualizar as informações do sistema de arquivos de acordo.<p>**Importante:** Até que você execute **chkdsk/b**, os clusters inválidos relatados em seu sistema recuperado podem não ser precisos. |
| -quiet | Executa o comando sem prompts para o usuário. |

## <a name="examples"></a>Exemplos

Para iniciar a recuperação das informações do backup que foi executado em 31 de março de 2020 às 9:00, localizada na unidade d:, digite:

```
wbadmin start sysrecovery -version:03/31/2020-09:00 -backupTarget:d:
```

Para iniciar a recuperação das informações do backup que foi executado em 30 de abril de 2020 às 9:00, localizada na pasta compartilhada `\\servername\share` para Server01, digite:

```
wbadmin start sysrecovery -version:04/30/2020-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [Get-WBBareMetalRecovery](/powershell/module/windowserverbackup/Get-WBBareMetalRecovery)
