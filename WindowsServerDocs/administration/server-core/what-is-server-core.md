---
title: O que é Server Core?
description: Saiba mais sobre a opção de instalação Server Core no Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885597"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>O que é a opção de instalação Server Core no Windows Server?

> Aplica-se a: Windows Server (canal semestral) e Windows Server 2016

A opção Server Core é uma opção de instalação mínima que está disponível quando você estiver implantando a edição Standard ou Datacenter do Windows Server. Server Core inclui a maioria, mas nem todas as funções de servidor. Server Core tem uma superfície menor do disco e, portanto, uma menor superfície de ataque devido a uma base de código menor. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Servidor (núcleo) versus servidor com experiência Desktop 
Quando você instala o Windows Server, você pode instalar apenas as funções de servidor que você escolher - isso ajuda a reduzir a superfície geral do Windows Server. No entanto, o opção servidor com experiência Desktop instalação ainda instala vários serviços e outros componentes que geralmente não são necessários para um cenário de uso específico. 

Isso é que o Server Core entra em cena: a instalação Server Core elimina todos os serviços e outros recursos que não são essenciais para o suporte de determinadas comumente usado funções de servidor. Por exemplo, um servidor Hyper-V não precisa de uma interface gráfica do usuário (GUI), porque você pode gerenciar praticamente todos os aspectos do Hyper-V da linha de comando usando o Windows PowerShell ou remotamente usando o Gerenciador do Hyper-V. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>A diferença do Server Core - principais recursos sem as sofisticações
Ao concluir a instalação Server Core em um sistema e entrar pela primeira vez, você está em um pouco surpreendente. A principal diferença entre o servidor com a opção de instalação Experiência Desktop e Server Core é que o Server Core não inclui os seguintes pacotes de shell da GUI:

- Microsoft-Windows-Server-Shell-Package
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

Em outras palavras, não há **área de trabalho não** no Server Core, por design. Mantendo os recursos necessários para dar suporte a aplicativos comerciais tradicionais e cargas de trabalho baseado em função, o Server Core não tem uma interface tradicional de área de trabalho. Em vez disso, o Server Core foi projetado para ser gerenciados remotamente por meio da linha de comando, PowerShell ou uma ferramenta de GUI (como [RSAT](../../remote/remote-server-administration-tools.md) ou [Windows Admin Center](../../manage/windows-admin-center/overview.md)).

Nenhuma interface do usuário, além de núcleo do servidor também é diferente do servidor com experiência Desktop das seguintes maneiras:

- Server Core não tem as ferramentas de acessibilidade
- Não há OOBE (out-de-inicial pelo usuário) para configurar o Server Core
- Não há suporte de áudio

A tabela a seguir mostra quais aplicativos estão disponíveis *localmente* no Server Core vs servidor com experiência Desktop. **Importante**: Na maioria dos casos, os aplicativos que são listados como "não disponível" abaixo pode ser executados remotamente em um computador de cliente do Windows e usados para gerenciar a instalação do Server Core.

> [!NOTE]
> Essa lista destina-se para referência rápida - ele não se destina a ser uma lista completa.


| Aplicativo                     | Server Core     | Servidor com Experiência Desktop |
|------------------------------------|-----------------|--------------------------------|
| Prompt de comando                     | disponível       | disponível                      |
| Windows PowerShell/ Microsoft .NET | disponível       | disponível                      |
| Perfmon.exe                        | não disponível  | disponível                      |
| Windbg (GUI)                         | compatível       | compatível                      |
| Resmon.exe                         | não disponível   | disponível                      |
| Regedit                            | disponível       | disponível                      |
| Fsutil.exe                         | disponível       | disponível                      |
| Disksnapshot.exe                   | não disponível   | disponível                      |
| Diskpart.exe                       | disponível       | disponível                      |
| Diskmgmt.msc                       | não disponível   | disponível                      |
| Devmgmt.msc                        | não disponível   | disponível                      |
| Gerenciador do Servidor                     | não disponível  | disponível                      |
| Mmc.exe                            | não disponível   | disponível                      |
| Eventvwr                           | não disponível  | disponível                      |
| Wevtutil (consultas de evento)           | disponível       | disponível                      |
| Services.msc                       | não disponível   | disponível                      |
| Painel de Controle                      | não disponível   | disponível                      |
| Windows Update (GUI)                 | não disponível | disponível                      |
| Windows Explorer                   | não disponível   | disponível                      |
| Barra de tarefas                            | não disponível   | disponível                      |
| Notificações da barra de tarefas              | não disponível   | disponível                      |
| Taskmgr                            | disponível       | disponível                      |
| Internet Explorer ou Microsoft Edge          | não disponível   | disponível                      |
| Sistema de ajuda integrado               | não disponível   | disponível                      |
| Windows 10 Shell                   | não disponível   | disponível                      |
| Windows Media Player               | não disponível   | disponível                      |
| PowerShell                         | disponível       | disponível                      |
| ISE do PowerShell                     | não disponível   | disponível                      |
| IME do PowerShell                     | disponível       | disponível                      |
| Mstsc.exe                          | não disponível   | disponível                      |
| Serviços da Área de Trabalho Remota            | disponível       | disponível                      |
| Gerenciador Hyper-V                    | não disponível  | disponível                      |


Para obter mais informações sobre o que *está* incluído no Server Core, consulte [funções, serviços de função e recursos incluídos no Windows Server - Server Core](server-core-roles-and-services.md). E para obter informações sobre o que *não é* incluído no Server Core, consulte [funções, serviços de função e recursos não incluídos no Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Introdução ao uso do Server Core
Use as informações a seguir para instalar, configurar e gerenciar a opção de instalação Server Core do Windows Server.

Instalação do Server Core: 
- [Funções, serviços de função e recursos incluídos no Server Core](server-core-roles-and-services.md)
- [Funções, serviços de função e recursos não estão no núcleo do servidor](server-core-removed-roles.md)
- [Instalar a opção de instalação Server Core](../../get-started/getting-started-with-server-core.md)
- [Configure o Server Core com a ferramenta de SConfig](../../get-started/sconfig-on-ws2016.md)

Uso do Server Core:
- [Tarefas básicas de administração do Server Core usando o Windows PowerShell ou a linha de comando](server-core-administer.md)
- [Gerenciar o Server Core](server-core-manage.md)
- [Aplicação de patch do Server Core](server-core-servicing.md)
- [Configurar arquivos de despejo de memória](server-core-memory-dump.md)