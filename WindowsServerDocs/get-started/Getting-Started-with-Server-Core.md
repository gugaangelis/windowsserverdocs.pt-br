---
title: Instalar o Server Core
description: Como obter e instalar uma instalação Server Core no Windows Server 2019, Windows Server 2016 ou Windows Server (canal semestral).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 1/04/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: d99cd0b028d08d5c3247541ce3a868676b60693d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869017"
---
# <a name="install-server-core"></a>Instalar o Server Core

> Aplica-se a: 2019, Windows Server 2016, Windows Server (canal semestral) do Windows Server
  
Quando você instala o Windows Server pela primeira vez, você tem as seguintes opções de instalação:

>[!NOTE]
> Na lista a seguir, as edições sem "Experiência Desktop" são as opções de instalação do Server Core

-   Windows Server Standard
-   Windows Server Standard com Experiência Desktop
-   Windows Server Datacenter
-   Windows Server Datacenter com Experiência Desktop

Quando você instala o Windows Server (canal semestral), incluindo versões 1709, 1803 e 1809, você tem as seguintes opções de instalação:

-   Windows Server Standard 
-   Windows Server Datacenter

A opção Server Core reduz o espaço necessário em disco e a superfície potencial de ataque, portanto recomendamos que você escolha a instalação Server Core, a menos que tenha uma necessidade particular dos elementos adicionais da interface do usuário e das ferramentas gráficas de gerenciamento que estão incluídas na opção Servidor com Experiência Desktop. Se você achar que precisa de elementos de interface do usuário adicionais, consulte [Install Server with Desktop Experience](Getting-Started-with-Server-with-Desktop-Experience.md) (Instalar o servidor com a experiência desktop). 

Com a opção Server Core, a interface padrão do usuário (a Experiência Desktop) não é instalada; você gerencia o servidor usando a linha de comando, o Windows PowerShell ou por métodos remotos.

>[!NOTE]
>
>Ao contrário de algumas versões anteriores do Windows Server, não é possível converter entre Server Core e Server com Experiência Desktop após a instalação. Se você instalar o Server Core e posteriormente decidir usar o Server com Experiência Desktop, deverá fazer uma nova instalação.

**Interface do usuário:** prompt de comando

**Instale, configure, desinstale funções de servidor localmente:** em um prompt de comando com o Windows PowerShell.

**Instale, configure, desinstale funções de servidor remotamente de um computador de cliente do Windows (ou um servidor com a experiência Desktop instalado):** com o Gerenciador do servidor, ferramentas de administração de servidor remoto (RSAT), Windows PowerShell ou Windows Admin Center .

>[!NOTE]
>
>Para as RSAT, é necessário usar a versão do Windows 10.
>O Console de Gerenciamento Microsoft não está disponível localmente.

**Funções de servidor de exemplo disponíveis:**

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

Para funções não incluídas no Server Core, consulte [funções, serviços de função e recursos não no Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>Instalação no Windows Server 2019 ou no Windows Server 2016

Para etapas de instalação geral e as opções para o Windows Server (canal de manutenção para longo prazo), consulte [instalação do Windows Server e atualização](installation-and-upgrade.md).

## <a name="installing-on-windows-server-semi-annual-channel"></a>Instalando no Windows Server (canal semestral)

Etapas de instalação do Windows Server (canal semestral) são o mesmo que a instalação de versões anteriores do Windows Server (de um. Imagem ISO), com as seguintes exceções:
- Nenhuma upgrade com suporte de versões anteriores do Windows Server para o Windows Server, versão 1709. Uma nova instalação sempre é necessária.
   Isso significa que quando você executa setup.exe da área de trabalho de um computador Windows, a experiência de configuração não permite que a opção de atualização (ele será esmaecido).
- Não há nenhuma versão de avaliação do Windows Server (canal semestral)
- Não há nenhum OEM ou uma versão de varejo. Windows Server (canal semestral) só podem ser licenciado por meio de programas de Software Assurance ou fidelidade.

Para obter o Windows Server versão 1709, consulte [Apresentando o Windows Server, versão 1709](get-started-with-1709.md).

Para obter a versão 1803 do Windows Server, consulte [apresentando o Windows Server, versão 1803](get-started-with-1803.md).

Para ver o que há de novo no Windows Server, versão 1809, consulte [que há de novo no Windows Server versão 1809](whats-new-in-windows-server-1809.md)
