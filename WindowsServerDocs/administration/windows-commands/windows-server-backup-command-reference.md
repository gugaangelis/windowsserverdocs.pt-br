---
title: Referência aos comandos do Backup do Windows Server
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03de0a65-21f0-4dd7-a3ae-251c98bbf6eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ded5039e122832c95eda864bcdcc76f580ca7108
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839807"
---
# <a name="windows-server-backup-command-reference"></a>Referência aos comandos do Backup do Windows Server



As seguintes subcomandos para **wbadmin** fornecem a funcionalidade de backup e recuperação em um prompt de comando.

Para configurar um agendamento de backup, você deve ser um membro do **administradores** grupo. Para executar todas as outras tarefas com este comando, você deve ser um membro do **operadores de Backup** ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas.

Você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um prompt de comando com privilégios elevados, clique em **inicie**, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador**.)

|Subcomando|Descrição|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|Configura e habilita um agendamento de backup diário.|
|[Desabilitar o WBADMIN backup](wbadmin-disable-backup.md)|Desabilita a seus backups diários.|
|[WBADMIN Iniciar backup](wbadmin-start-backup.md)|Executa um backup único. Se usado sem parâmetros, usa as configurações de agendamento de backup diário.|
|[Interrupção do trabalho de WBADMIN](wbadmin-stop-job.md)|Interrompe o atualmente executando o backup ou a operação de recuperação.|
|[versões do Wbadmin get](wbadmin-get-versions.md)|Lista detalhes de backups recuperáveis a partir do computador local ou, se outro local for especificado, de outro computador.|
|[Wbadmin get itens](wbadmin-get-items.md)|Lista os itens incluídos em um backup específico.|
|[WBADMIN iniciar recuperação](wbadmin-start-recovery.md)|Executa uma recuperação de volumes, aplicativos, arquivos ou pastas especificadas.|
|[obter o status da WBADMIN](wbadmin-get-status.md)|Mostra o status da operação de recuperação ou atualmente executando o backup.|
|[Wbadmin get discos](wbadmin-get-disks.md)|Lista os discos que estão atualmente online.|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Executa uma recuperação de estado do sistema.|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Executa um backup de estado do sistema.|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Exclui um ou mais backups de estado do sistema.|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Executa uma recuperação do sistema completo (pelo menos todos os volumes que contêm o estado do sistema operacional). Esse subcomando só estará disponível se você estiver usando o ambiente de recuperação do Windows.|
|[Catálogo de restauração de WBADMIN](wbadmin-restore-catalog.md)|Recupera um catálogo de backup de um local de armazenamento especificado no caso em que o catálogo de backup no computador local está corrompido.|
|[Catálogo de exclusão de WBADMIN](wbadmin-delete-catalog.md)|Exclui o catálogo de backup no computador local. Use este comando apenas se o catálogo de backup neste computador está corrompido e não há backups são armazenados em outro local que você pode usar para restaurar o catálogo.|