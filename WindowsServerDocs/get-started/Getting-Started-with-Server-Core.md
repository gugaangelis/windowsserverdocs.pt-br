---
title: Instalar o Server Core
description: Como obter e instalar uma instalação do Server Core no Windows Server 2019, Windows Server 2016 ou Windows Server (Canal Semestral).
ms.date: 05/21/2019
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 93b4cb477ce31543e67dd9f973637e830e0fd478
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87959784"
---
# <a name="install-server-core"></a>Instalar o Server Core

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Ao instalar o Windows Server 2016 pela primeira vez, você tem as seguintes opções de instalação:

>[!NOTE]
> Na lista a seguir, as edições sem "Experiência Desktop" são as opções de instalação do Server Core

-    Windows Server Standard
-    Windows Server Standard com Experiência Desktop
-    Windows Server Datacenter
-    Windows Server Datacenter com Experiência Desktop

Ao instalar o Windows Server 2016 (Canal Semestral) pela primeira vez, você tem as seguintes opções de instalação:

-    Windows Server Standard
-    Windows Server Datacenter

A opção Server Core reduz o espaço necessário em disco e a superfície de ataque potencial, portanto recomendamos que você escolha a instalação Server Core, a menos que tenha uma necessidade particular por elementos adicionais da interface do usuário e ferramentas gráficas de gerenciamento que estão incluídas no Server com a opção Experiência Desktop. Se você achar que precisa de elementos de interface do usuário adicionais, consulte [Instalar o Server com Experiência Desktop](Getting-Started-with-Server-with-Desktop-Experience.md).

Com a opção Server Core, a interface padrão do usuário (a Experiência Desktop) não é instalada. Você gerencia o servidor usando a linha de comando, o Windows PowerShell ou por métodos remotos.

>[!NOTE]
>
>Ao contrário de algumas versões anteriores do Windows Server, não é possível converter entre Server Core e Server com Experiência Desktop após a instalação. Se você instalar o Server Core e posteriormente decidir usar o Server com Experiência Desktop, será necessário fazer uma instalação limpa.

**Interface do usuário:** prompt de comando

**Instale, configure, desinstale funções de servidor localmente:** em um prompt de comando com o Windows PowerShell.

**Instale, configure, desinstale funções de servidor remotamente a partir de um computador de cliente do Windows (ou um servidor com a Experiência Desktop instalada:** com o Gerenciador do Servidor, RSAT (Ferramentas de Administração de Servidor Remoto), Windows PowerShell ou Windows Admin Center.

>[!NOTE]
>
>Para as RSAT, é necessário usar a versão do Windows 10.
>O Console de Gerenciamento Microsoft não está disponível localmente.

**Exemplo de funções de servidor disponíveis:**

- Serviços de Certificados do Active Directory
- Active Directory Domain Services
- Servidor DHCP
- Servidor DNS
- Serviços de Arquivo (incluindo o Gerenciador de Recursos do Servidor de Arquivos)
- AD LDS (Active Directory Lightweight Directory Services)
- Hyper-V
- Serviços de impressão e documentos
- Serviços de Mídia de Fluxo Contínuo
- Servidor Web (incluindo um sub conjunto de ASP.NET)
- Windows Server Update Services
- Servidor de Gerenciamento de Direitos do Active Directory
- Servidor de Roteamento e Acesso Remoto e as seguintes subfunções:
   - Agente de Conexão de Serviços de Área de Trabalho Remota
   - Licenciamento
   - Virtualização
   - Serviços de Ativação por Volume

Para funções não incluídas no Server Core, consulte [Funções, serviços de função e recursos não presentes no Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>Instalação no Windows Server 2019 ou no Windows Server 2016

Para ver as etapas de instalação geral e as opções para o Windows Server (Canal de Manutenção em Longo Prazo), consulte [Instalação e upgrade do Windows Server](installation-and-upgrade.md).

## <a name="installing-on-windows-server-semi-annual-channel"></a>Instalação no Windows Server (Canal Semestral)

As etapas de instalação do Windows Server (Canal Semestral) são as mesmas que para instalar versões anteriores do Windows Server (a partir de uma imagem .ISO), com as seguintes exceções:

- Não há suporte para atualizações de versões anteriores do Windows Server para o Windows Server, versão 1709. Uma instalação limpa é sempre necessária.
   Isso significa que, quando você executa o setup.exe na área de trabalho de um computador Windows, a experiência de instalação não permite a opção de upgrade (ela fica esmaecida).
- Não há nenhuma versão de avaliação para o Windows Server (Canal Semestral)
- Não há nenhuma versão de varejo ou OEM. O Windows Server (Canal Semestral) somente pode ser licenciado por meio do programa Software Assurance ou de programas de fidelidade.

Para obter mais informações sobre o Canal Semestral, consulte [Comparação dos canais de manutenção](../get-started-19/servicing-channels-19.md).

Para conhecer o que há de novo no Windows Server Canal Semestral, consulte [Novidades do Windows Server](whats-new-in-windows-server.md)
