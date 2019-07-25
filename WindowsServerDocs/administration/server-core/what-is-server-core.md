---
title: O que é o Server Core?
description: Saiba mais sobre a opção de instalação Server Core no Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: ce00bc973b7b750e33326cdec24193ba537b5294
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476473"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>O que é a opção de instalação Server Core no Windows Server?

> Aplica-se a: Windows Server 2019, Windows Server 2016 e Windows Server (canal semestral)

A opção Server Core é uma opção de instalação mínima que está disponível quando você está implantando a edição Standard ou Datacenter do Windows Server. O Server Core inclui a maioria das funções de servidor, mas não todas. O Server Core tem um espaço em disco menor e, portanto, uma superfície de ataque menor devido a uma base de código menor. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Servidor (núcleo) vs servidor com experiência desktop 
Ao instalar o Windows Server, você instala apenas as funções de servidor escolhidas – isso ajuda a reduzir a superfície geral do Windows Server. No entanto, a opção de instalação servidor com experiência desktop ainda instala muitos serviços e outros componentes que geralmente não são necessários para um cenário de uso específico. 

É aí que o Server Core entra em cena: a instalação Server Core elimina todos os serviços e outros recursos que não são essenciais para o suporte de certas funções de servidor usadas com frequência. Por exemplo, um servidor Hyper-V não precisa de uma GUI (interface gráfica do usuário), pois você pode gerenciar praticamente todos os aspectos do Hyper-V na linha de comando usando o Windows PowerShell ou remotamente usando o Gerenciador do Hyper-V. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>A principal diferença do servidor – principais recursos sem o sofisticações
Quando você terminar de instalar o Server Core em um sistema e entrar pela primeira vez, você estará em um pouco de surpresa. A principal diferença entre o servidor com a opção de instalação da experiência de desktop e o Server Core é que o Server Core não inclui os seguintes pacotes de Shell de GUI:

- Microsoft-Windows-Server-Shell-Package
- Microsoft-Windows-Server-GUI-MGMT-Package
- Microsoft-Windows-Server-GUI-RSAT-Package
- Microsoft-Windows-Cortana-PAL-desktop-Package

Em outras palavras, não há **área de trabalho** no Server Core, por design. Ao mesmo tempo em que mantém os recursos necessários para dar suporte a aplicativos de negócios tradicionais e cargas de trabalho baseadas em funções, o Server Core não tem uma interface de desktop tradicional. Em vez disso, o Server Core foi projetado para ser gerenciado remotamente por meio da linha de comando, do PowerShell ou de uma ferramenta de GUI (como o [rsat](../../remote/remote-server-administration-tools.md) ou o [centro de administração do Windows](../../manage/windows-admin-center/overview.md)).

Além de nenhuma interface do usuário, o Server Core também difere do servidor com a experiência desktop das seguintes maneiras:

- O Server Core não tem nenhuma ferramenta de acessibilidade
- Nenhum OOBE (pronto para uso) para configurar o Server Core
- Sem suporte a áudio

A tabela a seguir mostra quais aplicativos estão disponíveis *localmente* no Server Core vs Server with Desktop Experience. **Importante**: Na maioria dos casos, os aplicativos listados como "não disponíveis" abaixo podem ser executados remotamente de um computador cliente do Windows e usados para gerenciar a instalação do Server Core.

> [!NOTE]
> Esta lista destina-se à referência rápida-ela não se destina a ser uma lista completa.


| Aplicativo                     | Server Core     | Servidor com Experiência Desktop |
|------------------------------------|-----------------|--------------------------------|
| Prompt de comando                     | disponível       | disponível                      |
| Windows PowerShell/Microsoft .NET | disponível       | disponível                      |
| Perfmon. exe                        | não disponível  | disponível                      |
| WinDbg (GUI)                         | compatível       | compatível                      |
| Resmon. exe                         | não disponível   | disponível                      |
| Regedit                            | disponível       | disponível                      |
| Fsutil. exe                         | disponível       | disponível                      |
| Disksnapshot. exe                   | não disponível   | disponível                      |
| Diskpart.exe                       | disponível       | disponível                      |
| Diskmgmt. msc                       | não disponível   | disponível                      |
| Devmgmt. msc                        | não disponível   | disponível                      |
| Gerenciador do Servidor                     | não disponível  | disponível                      |
| MMC. exe                            | não disponível   | disponível                      |
| Eventvwr                           | não disponível  | disponível                      |
| Wevtutil (consultas de evento)           | disponível       | disponível                      |
| Services. msc                       | não disponível   | disponível                      |
| Painel de Controle                      | não disponível   | disponível                      |
| Windows Update (GUI)                 | não disponível | disponível                      |
| Windows Explorer                   | não disponível   | disponível                      |
| Barra de tarefas                            | não disponível   | disponível                      |
| Notificações da barra de tarefas              | não disponível   | disponível                      |
| Taskmgr                            | disponível       | disponível                      |
| Internet Explorer ou Edge          | não disponível   | disponível                      |
| Sistema de ajuda integrado               | não disponível   | disponível                      |
| Shell do Windows 10                   | não disponível   | disponível                      |
| Windows Media Player               | não disponível   | disponível                      |
| PowerShell                         | disponível       | disponível                      |
| ISE do PowerShell                     | não disponível   | disponível                      |
| IME do PowerShell                     | disponível       | disponível                      |
| Mstsc. exe                          | não disponível   | disponível                      |
| Serviços da Área de Trabalho Remota            | disponível       | disponível                      |
| Gerenciador Hyper-V                    | não disponível  | disponível                      |


Para obter mais informações sobre o que *está* incluído no Server Core, consulte [funções, serviços de função e recursos incluídos no Windows Server-Server Core](server-core-roles-and-services.md). E para obter informações sobre o que *não está* incluído no Server Core, consulte [funções, serviços de função e recursos não incluídos no Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Introdução ao uso do Server Core
Use as informações a seguir para instalar, configurar e gerenciar a opção de instalação Server Core do Windows Server.

Instalação do Server Core: 
- [Funções, serviços de função e recursos incluídos no Server Core](server-core-roles-and-services.md)
- [Funções, serviços de função e recursos que não estão no Server Core](server-core-removed-roles.md)
- [Instalar a opção de instalação Server Core](../../get-started/getting-started-with-server-core.md)
- [Configurar o Server Core com a ferramenta SConfig](../../get-started/sconfig-on-ws2016.md)

Usando o Server Core:
- [Tarefas básicas de administração do Server Core usando o Windows PowerShell ou a linha de comando](server-core-administer.md)
- [Gerenciar o Server Core](server-core-manage.md)
- [Núcleo do servidor de aplicação de patch](server-core-servicing.md)
- [Configurar arquivos de despejo de memória](server-core-memory-dump.md)