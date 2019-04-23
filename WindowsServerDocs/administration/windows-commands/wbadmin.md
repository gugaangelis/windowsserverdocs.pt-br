---
title: wbadmin
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ce8109ef6a0885abd02ef1dee9f11d21b7d7e9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858877"
---
# <a name="wbadmin"></a>wbadmin



Permite fazer backup e restaurar seu sistema operacional, volumes, arquivos, pastas e aplicativos em um prompt de comando.

Para configurar um backup agendado regularmente, você deve ser um membro do **administradores** grupo. Para executar todas as outras tarefas com este comando, você deve ser um membro do **operadores de Backup** ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas.

Você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um prompt de comando com privilégios elevados, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador**.)

## <a name="subcommands"></a>Subcomandos

|Subcomando|Descrição|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|Configura e permite que um backup agendado regularmente.|
|[Desabilitar o WBADMIN backup](wbadmin-disable-backup.md)|Desabilita a seus backups diários.|
|[WBADMIN Iniciar backup](wbadmin-start-backup.md)|Executa um backup único. Se usado sem parâmetros, usa as configurações de agendamento de backup diário.|
|[Interrupção do trabalho de WBADMIN](wbadmin-stop-job.md)|Interrompe o atualmente executando o backup ou a operação de recuperação.|
|[versões do Wbadmin get](wbadmin-get-versions.md)|Lista detalhes de backups recuperáveis a partir do computador local ou, se outro local for especificado, de outro computador.|
|[Wbadmin get itens](wbadmin-get-items.md)|Lista os itens incluídos em um backup.|
|[WBADMIN iniciar recuperação](wbadmin-start-recovery.md)|Executa uma recuperação de volumes, aplicativos, arquivos ou pastas especificadas.|
|[obter o status da WBADMIN](wbadmin-get-status.md)|Mostra o status da operação de recuperação ou atualmente executando o backup.|
|[Wbadmin get discos](wbadmin-get-disks.md)|Lista os discos que estão atualmente online.|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Executa uma recuperação de estado do sistema.|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Executa um backup de estado do sistema.|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Exclui um ou mais backups de estado do sistema.|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Executa uma recuperação do sistema completo (pelo menos todos os volumes que contêm o estado do sistema operacional). Esse subcomando só estará disponível se você estiver usando o ambiente de recuperação do Windows.|
|[Catálogo de restauração de WBADMIN](wbadmin-restore-catalog.md)|Recupera um catálogo de backup de um local de armazenamento especificado no caso em que o catálogo de backup no computador local está corrompido.|
|[Catálogo de exclusão de WBADMIN](wbadmin-delete-catalog.md)|Exclui o catálogo de backup no computador local. Use este subcomando somente se o catálogo de backup neste computador está corrompido e não há backups são armazenados em outro local que você pode usar para restaurar o catálogo.|

#### <a name="additional-references"></a>Referências adicionais

-   [Backup e recuperação](https://go.microsoft.com/fwlink/?LinkID=195054)
-   [Cmdlets de Backup do Windows Server no Windows PowerShell](https://technet.microsoft.com/library/jj902428.aspx)