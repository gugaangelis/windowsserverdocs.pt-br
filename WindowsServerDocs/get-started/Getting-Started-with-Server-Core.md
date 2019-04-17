---
title: Instalar o Server Core
description: Como obter e instalar uma instalação Server Core no Windows Server (canal semestral), Windows Server 2016 ou Windows Server 2019.
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
ms.sourcegitcommit: 7fc7271745e40f110c54918b55624cadd0d7ff98
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/04/2019
ms.locfileid: "8991793"
---
# Instalar o Server Core

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)
  
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

**Instalar, configurar, desinstale funções de servidor remotamente de um computador de cliente do Windows (ou um servidor com a experiência Desktop instalado):** com o Gerenciador do servidor, ferramentas de administração de servidor remoto (RSAT), Windows PowerShell ou o Windows Admin Center.

>[!NOTE]
>
>Para RSAT, é necessário usar a versão do Windows 10.
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
- Serviços de ativação de volume

Para funções não incluídas no Server Core, consulte [funções, serviços de função e recursos não no Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## Instalar no Windows Server 2019 ou Windows Server 2016

Para as etapas gerais de instalação e opções para o Windows Server (canal de manutenção a longo prazo), consulte a [atualização e instalação do Windows Server](installation-and-upgrade.md).

## Instalando no Windows Server (canal semestral)

Etapas de instalação do Windows Server (canal semestral) são as mesmas para instalar versões anteriores do Windows Server (de um. Imagem ISO), com as seguintes exceções:
- Nenhuma upgrade com suporte de versões anteriores do Windows Server para o Windows Server, versão 1709. Uma nova instalação sempre é necessária.
   Isso significa que, quando você executa setup.exe da área de trabalho de um computador Windows, a experiência de instalação não permite a opção de upgrade (ela está desativada).
- Não há nenhuma versão de avaliação do Windows Server (canal semestral)
- Não há nenhum OEM ou uma versão de varejo. Windows Server (canal semestral) pode ser licenciado somente por meio de programas de Software Assurance ou de fidelidade.

Para obter o Windows Server versão 1709, consulte [Apresentando o Windows Server, versão 1709](get-started-with-1709.md).

Para obter o Windows Server versão 1803, consulte [Apresentando o Windows Server, versão 1803](get-started-with-1803.md).

Para ver o que há de novo no Windows Server, versão 1809, veja [as novidades no Windows Server versão 1809](whats-new-in-windows-server-1809.md)
