---
title: wbadmin
description: Tópico de referência para o Wbadmin, que permite fazer backup e restaurar seu sistema operacional, volumes, arquivos, pastas e aplicativos de um prompt de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94f07d17d46dad4e5301ba3ea6be94b10f26a3af
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725832"
---
# <a name="wbadmin"></a>wbadmin



Permite fazer backup e restaurar seu sistema operacional, volumes, arquivos, pastas e aplicativos de um prompt de comando.

Para configurar um backup agendado regularmente, você deve ser um membro do grupo **Administradores** . Para executar todas as outras tarefas com este comando, você deve ser um membro do grupo **operadores de backup** ou **Administradores** , ou você deve ter recebido as permissões apropriadas.

Você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

## <a name="subcommands"></a>Subcomandos

|Subcomando|Descrição|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|Configura e habilita um backup agendado regularmente.|
|[Wbadmin disable backup](wbadmin-disable-backup.md)|Desabilita os backups diários.|
|[Wbadmin start backup](wbadmin-start-backup.md)|Executa um backup único. Se usado sem parâmetros, o usa as configurações do agendamento de backup diário.|
|[Wbadmin stop job](wbadmin-stop-job.md)|Interrompe a operação de backup ou recuperação em execução no momento.|
|[Wbadmin get versions](wbadmin-get-versions.md)|Lista detalhes de backups recuperáveis do computador local ou, se outro local for especificado, de outro computador.|
|[Wbadmin get items](wbadmin-get-items.md)|Lista os itens incluídos em um backup.|
|[Wbadmin start recovery](wbadmin-start-recovery.md)|Executa uma recuperação de volumes, aplicativos, arquivos ou pastas especificadas.|
|[Wbadmin get status](wbadmin-get-status.md)|Mostra o status da operação de backup ou recuperação em execução no momento.|
|[Wbadmin get disks](wbadmin-get-disks.md)|Lista os discos que estão online no momento.|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Executa uma recuperação de estado do sistema.|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Executa um backup de estado do sistema.|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Exclui um ou mais backups de estado do sistema.|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Executa uma recuperação do sistema completo (pelo menos todos os volumes que contêm o estado do sistema operacional). Esse subcomando só estará disponível se você estiver usando o ambiente de recuperação do Windows.|
|[Wbadmin restore catalog](wbadmin-restore-catalog.md)|Recupera um catálogo de backup de um local de armazenamento especificado no caso em que o catálogo de backup no computador local está corrompido.|
|[Wbadmin delete catalog](wbadmin-delete-catalog.md)|Exclui o catálogo de backup no computador local. Use esse subcomando somente se o catálogo de backup deste computador estiver corrompido e você não tiver nenhum backup armazenado em outro local que possa ser usado para restaurar o catálogo.|

## <a name="additional-references"></a>Referências adicionais

-   [Backup e recuperação](https://go.microsoft.com/fwlink/?LinkID=195054)
-   [Backup do Windows Server cmdlets no Windows PowerShell](https://technet.microsoft.com/library/jj902428.aspx)