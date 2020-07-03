---
title: Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)
description: Artigo de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55466409517b63c52f88a7acec3a8f4aba7d258d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933476"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Veja a seguir uma lista de Serviços de Área de Trabalho Remota ferramentas de linha de comando.
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
>
> |                 Comando                 |                                                      Descrição                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | altera as configurações de servidor Host da Sessão da Área de Trabalho Remota (host de Sessão RD) para logons, mapeamentos de porta COM e modo de instalação. |
> |     [change logon](change-logon.md)     |    Habilita ou desabilita os logons de sessões de cliente em um servidor de host de sessão de área de trabalho remota ou exibe o status de logon atual.     |
> |      [change port](change-port.md)      |                   lista ou altera os mapeamentos de porta COM para que sejam compatíveis com os aplicativos do MS-DOS.                    |
> |      [change user](change-user.md)      |                                altera o modo de instalação do servidor de host da sessão da área de trabalho remota.                                |
> |         [chglogon](chglogon.md)         |    Habilita ou desabilita os logons de sessões de cliente em um servidor de host de sessão de área de trabalho remota ou exibe o status de logon atual.     |
> |          [chgport](chgport.md)          |                   lista ou altera os mapeamentos de porta COM para que sejam compatíveis com os aplicativos do MS-DOS.                    |
> |           [chgusr](chgusr.md)           |                                altera o modo de instalação do servidor de host da sessão da área de trabalho remota.                                |
> |         [flattemp](flattemp.md)         |                                      Habilita ou desabilita pastas temporárias simples.                                       |
> |           [logoff](logoff.md)           |          Faz logoff de um usuário de uma sessão em um servidor de host de sessão de área de trabalho remota e exclui a sessão do servidor.          |
> |              [msg](msg.md)              |                                Envia uma mensagem para um usuário em um servidor de host de sessão de área de trabalho remota.                                 |
> |            [mstsc](mstsc.md)            |                       Cria conexões com servidores de host de sessão da área de trabalho remota ou outros computadores remotos.                        |
> |          [qappsrv](qappsrv.md)          |                             Exibe uma lista de todos os servidores de host da sessão da área de trabalho remota na rede.                             |
> |         [qprocess](qprocess.md)         |                  Exibe informações sobre os processos que estão sendo executados em um servidor de host de sessão de área de trabalho remota.                   |
> |            [consulta](query.md)            |                      Exibe informações sobre processos, sessões e servidores host da Sessão RD.                      |
> |    [query process](query-process.md)    |                  Exibe informações sobre os processos que estão sendo executados em um servidor de host de sessão de área de trabalho remota.                   |
> |    [query session](query-session.md)    |                           Exibe informações sobre sessões em um servidor de host de sessão de área de trabalho remota.                            |
> | [query termserver](query-termserver.md) |                             Exibe uma lista de todos os servidores de host da sessão da área de trabalho remota na rede.                             |
> |       [query user](query-user.md)       |                         Exibe informações sobre sessões de usuário em um servidor de host de sessão de área de trabalho remota.                         |
> |            [quser](quser.md)            |                         Exibe informações sobre sessões de usuário em um servidor de host de sessão de área de trabalho remota.                         |
> |          [qwinsta](qwinsta.md)          |                           Exibe informações sobre sessões em um servidor de host de sessão de área de trabalho remota.                            |
> |          [rdpsign](rdpsign.md)          |                          Permite que você assine digitalmente um arquivo protocolo RDP (. RDP).                          |
> |    [reset session](reset-session.md)    |                         Permite redefinir (excluir) uma sessão em um servidor de host de sessão de área de trabalho remota.                          |
> |          [rwinsta](rwinsta.md)          |                         Permite redefinir (excluir) uma sessão em um servidor de host de sessão de área de trabalho remota.                          |
> |           [shadow](shadow.md)           |            Permite controlar remotamente uma sessão ativa de outro usuário em um servidor de host de sessão de área de trabalho remota.             |
> |            [tscon](tscon.md)            |                               Conecta-se a outra sessão em um servidor de host de sessão de área de trabalho remota.                                |
> |         [tsdiscon](tsdiscon.md)         |                                 Desconecta uma sessão de um servidor de host de sessão de área de trabalho remota.                                  |
> |           [tskill](tskill.md)           |                           Finaliza um processo em execução em uma sessão em um servidor de host de sessão de área de trabalho remota.                            |
> |           [tsprof](tsprof.md)           |              Copia as informações de configuração de usuário Serviços de Área de Trabalho Remota de um usuário para outro.               |
