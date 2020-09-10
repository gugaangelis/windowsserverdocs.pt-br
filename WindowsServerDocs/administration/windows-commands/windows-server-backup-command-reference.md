---
title: Referência aos comandos do Backup do Windows Server
description: Artigo de referência para referência de comando de backup.
ms.topic: reference
ms.assetid: 03de0a65-21f0-4dd7-a3ae-251c98bbf6eb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7896769381b56e2553ac6566297d8b0aee8c2686
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641161"
---
# <a name="windows-server-backup-command-reference"></a>Referência aos comandos do Backup do Windows Server



Os seguintes subcomandos para **Wbadmin** fornecem a funcionalidade de backup e recuperação de um prompt de comando.

Para configurar um agendamento de backup, você deve ser um membro do grupo **Administradores** . Para executar todas as outras tarefas com este comando, você deve ser um membro do grupo **operadores de backup** ou **Administradores** , ou você deve ter recebido as permissões apropriadas.

Você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique em **Iniciar**, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

|Subcomando|Descrição|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|Configura e habilita um agendamento de backup diário.|
|[Wbadmin disable backup](wbadmin-disable-backup.md)|Desabilita os backups diários.|
|[Wbadmin start backup](wbadmin-start-backup.md)|Executa um backup único. Se usado sem parâmetros, o usa as configurações do agendamento de backup diário.|
|[Wbadmin stop job](wbadmin-stop-job.md)|Interrompe a operação de backup ou recuperação em execução no momento.|
|[Wbadmin get versions](wbadmin-get-versions.md)|Lista detalhes de backups recuperáveis do computador local ou, se outro local for especificado, de outro computador.|
|[Wbadmin get items](wbadmin-get-items.md)|Lista os itens incluídos em um backup específico.|
|[Wbadmin start recovery](wbadmin-start-recovery.md)|Executa uma recuperação de volumes, aplicativos, arquivos ou pastas especificadas.|
|[Wbadmin get status](wbadmin-get-status.md)|Mostra o status da operação de backup ou recuperação em execução no momento.|
|[Wbadmin get disks](wbadmin-get-disks.md)|Lista os discos que estão online no momento.|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Executa uma recuperação de estado do sistema.|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Executa um backup de estado do sistema.|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Exclui um ou mais backups de estado do sistema.|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Executa uma recuperação do sistema completo (pelo menos todos os volumes que contêm o estado do sistema operacional). Esse subcomando só estará disponível se você estiver usando o ambiente de recuperação do Windows.|
|[Wbadmin restore catalog](wbadmin-restore-catalog.md)|Recupera um catálogo de backup de um local de armazenamento especificado no caso em que o catálogo de backup no computador local está corrompido.|
|[Wbadmin delete catalog](wbadmin-delete-catalog.md)|Exclui o catálogo de backup no computador local. Use este comando somente se o catálogo de backup neste computador estiver corrompido e você não tiver nenhum backup armazenado em outro local que possa ser usado para restaurar o catálogo.|