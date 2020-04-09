---
title: Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2e188be84c657688a971a75788942d4acf598d7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836059"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Veja a seguir uma lista de Serviços de Área de Trabalho Remota ferramentas de linha de comando.
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
> 
> |                 {1&gt;Comando&lt;1}                 |                                                      Descrição                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | altera as configurações de servidor Host da Sessão da Área de Trabalho Remota (host de Sessão RD) para logons, mapeamentos de porta COM e modo de instalação. |
> |     [change logon](change-logon.md)     |    Habilita ou desabilita os logons de sessões de cliente em um servidor de host de sessão de área de trabalho remota ou exibe o status de logon atual.     |
> |      [change port](change-port.md)      |                   Lista ou altera os mapeamentos de porta COM para que sejam compatíveis com os aplicativos do MS-DOS.                    |
> |      [change user](change-user.md)      |                                altera o modo de instalação do servidor de host da sessão da área de trabalho remota.                                |
> |         [chglogon](chglogon.md)         |    Habilita ou desabilita os logons de sessões de cliente em um servidor de host de sessão de área de trabalho remota ou exibe o status de logon atual.     |
> |          [chgport](chgport.md)          |                   Lista ou altera os mapeamentos de porta COM para que sejam compatíveis com os aplicativos do MS-DOS.                    |
> |           [chgusr](chgusr.md)           |                                altera o modo de instalação do servidor de host da sessão da área de trabalho remota.                                |
> |         [flattemp](flattemp.md)         |                                      Habilita ou desabilita pastas temporárias simples.                                       |
> |           [logoff](logoff.md)           |          Faz logoff de um usuário de uma sessão em um servidor de host de sessão de área de trabalho remota e exclui a sessão do servidor.          |
> |              [msg](msg.md)              |                                Envia uma mensagem para um usuário em um servidor de host de sessão de área de trabalho remota.                                 |
> |            [mstsc](mstsc.md)            |                       Cria conexões com servidores de host de sessão da área de trabalho remota ou outros computadores remotos.                        |
> |          [qappsrv](qappsrv.md)          |                             Exibe uma lista de todos os servidores de host da sessão da área de trabalho remota na rede.                             |
> |         [qprocess](qprocess.md)         |                  Exibe informações sobre os processos que estão sendo executados em um servidor de host de sessão de área de trabalho remota.                   |
> |            [consulta](query.md)            |                      Exibe informações sobre processos, sessões e servidores host da Sessão RD.                      |
> |    [processo de consulta](query-process.md)    |                  Exibe informações sobre os processos que estão sendo executados em um servidor de host de sessão de área de trabalho remota.                   |
> |    [sessão de consulta](query-session.md)    |                           Exibe informações sobre sessões em um servidor de host de sessão de área de trabalho remota.                            |
> | [termserver de consulta](query-termserver.md) |                             Exibe uma lista de todos os servidores de host da sessão da área de trabalho remota na rede.                             |
> |       [consultar usuário](query-user.md)       |                         Exibe informações sobre sessões de usuário em um servidor de host de sessão de área de trabalho remota.                         |
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
