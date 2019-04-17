---
title: O que é o núcleo do servidor?
description: Saiba mais sobre a opção de instalação Server Core no Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718530"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>Qual é a opção de instalação Server Core no Windows Server?

> Aplica-se a: Windows Server (canal delimitadas anual) e Windows Server 2016

A opção Server Core é uma opção de instalação mínimos que está disponível quando você estiver implantando a edição Standard ou Datacenter do Windows Server. A maioria dos, mas nem todas as funções de servidor que inclui o núcleo do servidor. Núcleo do servidor tem um espaço de disco menor e, portanto, uma superfície de ataque menor devido a uma base de código menor. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Servidor (núcleo) versus servidor com experiência de área de trabalho 
Quando você instala o Windows Server, você pode instalar apenas as funções de servidor que você escolher - isso ajuda a reduzir a superfície geral para o Windows Server. No entanto, o servidor com a opção de instalação do recurso Experiência Desktop ainda instala vários serviços e outros componentes que geralmente não são necessários para um cenário de uso específico. 

Isso é onde o núcleo do servidor entra em ação: a instalação Server Core elimina quaisquer serviços e outros recursos que não são essenciais para o suporte de determinadas comumente usadas funções de servidor. Por exemplo, um servidor Hyper-V não precisa de uma interface gráfica do usuário (GUI), porque você pode gerenciar praticamente todos os aspectos do Hyper-V na linha de comando usando o Windows PowerShell ou remotamente usando o Gerenciador de Hyper-V. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>A diferença de núcleo do servidor - principais recursos sem os sofisticações
Quando você concluir a instalação Server Core em um sistema e entrar pela primeira vez, você está em um pouco de diante de surpresas. A principal diferença entre o servidor com a opção de instalação de experiência de área de trabalho e núcleo do servidor é que o núcleo do servidor não inclui os seguintes pacotes shell GUI:

- Microsoft-Windows-Shell-pacote de servidor
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

Em outras palavras, não há **nenhuma área de trabalho** no núcleo do servidor, por design. Mantendo as capacidades necessárias para dar suporte a aplicativos comerciais tradicionais e cargas de trabalho baseado em função, núcleo do servidor não tem uma interface de mesa tradicional. Em vez disso, o núcleo do servidor foi projetado para ser gerenciado remotamente por meio da linha de comando, PowerShell ou uma ferramenta GUI (como [RSAT](../../remote/remote-server-administration-tools.md) ou [Centro de administração do Windows](../../manage/windows-admin-center/overview.md)).

Nenhuma interface de usuário, além de núcleo do servidor também difere no servidor com experiência de área de trabalho das seguintes maneiras:

- Núcleo do servidor não tem quaisquer ferramentas de acessibilidade
- Nenhum OOBE (out-de-caixa-experiência) para configurar o núcleo do servidor
- Não há suporte a áudio

A tabela a seguir mostra quais aplicativos estão disponíveis *localmente* no servidor principal vs Server com experiência de área de trabalho. **Importante**: na maioria dos casos, os aplicativos que estão listados, como "não disponível" abaixo pode ser executados remotamente de um computador de cliente do Windows e usados para gerenciar sua instalação Server Core.

> [!NOTE]
> Esta lista destina-se de referência rápida - ele não é projetado para ser uma lista completa.


| Aplicativo                     | Server Core     | Servidor com Experiência Desktop |
|------------------------------------|-----------------|--------------------------------|
| Prompt de comando                     | disponível       | disponível                      |
| O Windows PowerShell / Microsoft .NET | disponível       | disponível                      |
| Perfmon.exe                        | não disponível  | disponível                      |
| WinDBG (GUI)                         | com suporte       | com suporte                      |
| Resmon.exe                         | não disponível   | disponível                      |
| Regedit                            | disponível       | disponível                      |
| Fsutil.exe                         | disponível       | disponível                      |
| Disksnapshot.exe                   | não disponível   | disponível                      |
| Diskpart.exe                       | disponível       | disponível                      |
| Diskmgmt. msc                       | não disponível   | disponível                      |
| Devmgmt. msc                        | não disponível   | disponível                      |
| Gerenciador do Servidor                     | não disponível  | disponível                      |
| MMC.exe                            | não disponível   | disponível                      |
| Eventvwr                           | não disponível  | disponível                      |
| Wevtutil (consultas de evento)           | disponível       | disponível                      |
| Services. msc                       | não disponível   | disponível                      |
| Painel de Controle                      | não disponível   | disponível                      |
| Atualização do Windows (GUI)                 | não disponível | disponível                      |
| Windows Explorer                   | não disponível   | disponível                      |
| Barra de tarefas                            | não disponível   | disponível                      |
| Notificações da barra de tarefas              | não disponível   | disponível                      |
| Taskmgr                            | disponível       | disponível                      |
| Internet Explorer ou borda          | não disponível   | disponível                      |
| Sistema de Ajuda interna               | não disponível   | disponível                      |
| Shell do Windows 10                   | não disponível   | disponível                      |
| Windows Media Player               | não disponível   | disponível                      |
| PowerShell                         | disponível       | disponível                      |
| PowerShell ISE                     | não disponível   | disponível                      |
| IME do PowerShell                     | disponível       | disponível                      |
| Mstsc.exe                          | não disponível   | disponível                      |
| Serviços de Área de Trabalho Remota            | disponível       | disponível                      |
| Gerenciador do Hyper-V                    | não disponível  | disponível                      |


Para obter mais informações sobre o que *é* incluído no núcleo do servidor, consulte [funções, serviços de função e recursos incluídos no Windows Server - núcleo do servidor](server-core-roles-and-services.md). E para obter informações sobre quais *não está* incluído no núcleo do servidor, consulte [funções, serviços de função e recursos não são incluídos no núcleo do servidor](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Começar a usar o núcleo do servidor
Use as informações a seguir para instalar, configurar e gerenciar a opção de instalação Server Core do Windows Server.

Instalação do núcleo do servidor: 
- [Funções, serviços de função e recursos incluídos no núcleo do servidor](server-core-roles-and-services.md)
- [Funções, serviços de função e recursos que não núcleo do servidor](server-core-removed-roles.md)
- [Instalar a opção de instalação Server Core](../../get-started/getting-started-with-server-core.md)
- [Configurar o núcleo do servidor com a ferramenta de SConfig](../../get-started/sconfig-on-ws2016.md)

Usando o núcleo do servidor:
- [Tarefas de administração básicas núcleo do servidor usando o Windows PowerShell ou a linha de comando](server-core-administer.md)
- [Gerenciar o núcleo do servidor](server-core-manage.md)
- [Aplicando o patch de núcleo do servidor](server-core-servicing.md)
- [Configurar os arquivos de despejo de memória](server-core-memory-dump.md)