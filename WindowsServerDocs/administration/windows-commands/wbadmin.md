---
title: wbadmin
description: Artigo de referência para os comandos Wbadmin, que permite fazer backup e restaurar seu sistema operacional, volumes, arquivos, pastas e aplicativos a partir de um prompt de comando.
ms.topic: reference
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: df96e85d7cb4999088efe9bd6ede70087cd24cb7
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524661"
---
# <a name="wbadmin"></a>wbadmin

Permite fazer backup e restaurar seu sistema operacional, volumes, arquivos, pastas e aplicativos de um prompt de comando.

Para configurar um backup agendado regularmente usando esse comando, você deve ser um membro do grupo **Administradores** . Para executar todas as outras tarefas com este comando, você deve ser um membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas.

Você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

## <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [wbadmin delete catalog](wbadmin-delete-catalog.md) | Exclui o catálogo de backup no computador local. Use este comando somente se o catálogo de backup neste computador estiver corrompido e você não tiver nenhum backup armazenado em outro local que possa ser usado para restaurar o catálogo. |
| [wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md) | Exclui um ou mais backups de estado do sistema. |
| [wbadmin disable backup](wbadmin-disable-backup.md) | Desabilita os backups diários. |
| [wbadmin enable backup](wbadmin-enable-backup.md) | Configura e habilita um backup agendado regularmente. |
| [wbadmin get disks](wbadmin-get-disks.md) | Lista os discos que estão online no momento. |
| [wbadmin get items](wbadmin-get-items.md) | Lista os itens incluídos em um backup. |
| [wbadmin get status](wbadmin-get-status.md) | Mostra o status da operação de backup ou recuperação em execução no momento. |
| [wbadmin get versions](wbadmin-get-versions.md) | Lista detalhes de backups recuperáveis do computador local ou, se outro local for especificado, de outro computador. |
| [wbadmin restore catalog](wbadmin-restore-catalog.md) | Recupera um catálogo de backup de um local de armazenamento especificado no caso em que o catálogo de backup no computador local está corrompido. |
| [wbadmin start backup](wbadmin-start-backup.md) | Executa um backup único. Se usado sem parâmetros, o usa as configurações do agendamento de backup diário. |
| [wbadmin start recovery](wbadmin-start-recovery.md) | Executa uma recuperação de volumes, aplicativos, arquivos ou pastas especificadas. |
| [wbadmin start sysrecovery](wbadmin-start-sysrecovery.md) | Executa uma recuperação do sistema completo (pelo menos todos os volumes que contêm o estado do sistema operacional). Esse comando só estará disponível se você estiver usando o ambiente de recuperação do Windows. |
| [wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md) | Executa um backup de estado do sistema. |
| [wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md) | Executa uma recuperação de estado do sistema. |
| [wbadmin stop job](wbadmin-stop-job.md) | Interrompe a operação de backup ou recuperação em execução no momento. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Backup do Windows Server cmdlets no Windows PowerShell](/powershell/module/windowsserverbackup)

- [Ambiente de recuperação do Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
