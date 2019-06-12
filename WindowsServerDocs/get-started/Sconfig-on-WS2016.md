---
title: Configurar uma instalação Server Core do Windows Server com Sconfig.cmd
description: Explica como usar Sconfig.cmd
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e6cac074-c6fc-46dd-9664-fa0342c0a5e8
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 617005fd2d4e63c3cfc11bed28404656b2a81d6e
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749590"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>Configurar uma instalação Server Core do Windows Server 2016 ou Windows Server, versão 1709, com Sconfig.cmd

> Aplica-se a: Windows Server (canal semestral) e Windows Server 2016

No Windows Server 2016, Windows Server, versão 1709, você pode usar a ferramenta de Configuração do Servidor (Sconfig.cmd) para configurar e gerenciar vários aspectos comuns de instalações Server Core. Você deve ser membro do grupo Administradores para usar a ferramenta.

Você pode usar o Sconfig.cmd nas instalações Server Core e Server com Experiência Desktop (somente para Windows Server 2016).

## <a name="start-the-server-configuration-tool"></a>Iniciar a ferramenta de Configuração do Servidor

1. Mude para a unidade do sistema.

2. Digite `Sconfig.cmd` e pressione ENTER. A interface da ferramenta de Configuração do Servidor é aberta:

    ![Captura de tela da interface do usuário de Sconfig.cmd](media/mainsconfigpage.png)

## <a name="domainworkgroup-settings"></a>Configurações de domínio/grupo de trabalho

As configurações atuais de domínio/grupo de trabalho são exibidas na tela padrão da ferramenta de Configuração do Servidor. Você pode ingressar em um domínio ou um grupo de trabalho, acessando o **domínio/grupo de trabalho** necessária de página de configurações no menu principal e seguindo as instruções, fornecendo as informações.

Se um usuário de domínio não tenha sido adicionado ao grupo Administradores Local, você não poderá fazer alterações no sistema, como alterar o nome do computador, usando-se o usuário de domínio. Para adicionar um usuário do domínio no grupo de administradores local, permita que o computador seja reiniciado. Em seguida, faça logon computador como administrador local e siga as etapas a [configurações de Administrador Local](#local-administrator-settings) seção mais adiante neste artigo.

> [!NOTE]
> Você precisa reiniciar o servidor para aplicar as alterações à associação de domínio ou grupo de trabalho. No entanto, você pode fazer outras alterações e reiniciar o servidor depois de todas as mudanças para não precisar reiniciar o servidor várias vezes. Por padrão, as máquinas virtuais em execução são salvas automaticamente antes de reiniciar o Hyper-V Server.

## <a name="computer-name-settings"></a>Configurações de nome do computador

O nome do computador atual é exibido na tela padrão da Ferramenta de Configuração do Servidor. Você pode alterar o nome do computador, acessando o **nome do computador** página de configurações no menu principal e seguindo as instruções.

> [!NOTE]
> Você precisa reiniciar o servidor para aplicar as alterações à associação de domínio ou grupo de trabalho. No entanto, você pode fazer outras alterações e reiniciar o servidor depois de todas as mudanças para não precisar reiniciar o servidor várias vezes. Por padrão, as máquinas virtuais em execução são salvas automaticamente antes de reiniciar o Hyper-V Server.

## <a name="local-administrator-settings"></a>Configurações de administrador local

Para adicionar usuários ao grupo local de administradores, utilize a opção **Adicionar Administrador Local** no menu principal. Em um computador ingressado no domínio, insira o usuário no seguinte formato: domínio\nome de usuário. Em um computador que não ingressou no domínio (computador de grupo de trabalho), insira apenas o nome de usuário. As alterações entram em vigor imediatamente.

## <a name="network-settings"></a>Configurações da rede

Você pode configurar o endereço IP a ser atribuído automaticamente por um Servidor DHCP ou atribuir um endereço IP estático manualmente. Esta opção permite que você defina as configurações do Servidor DNS para o servidor também.

> [!NOTE]
> Estas opções e muitas mais estão agora disponíveis com os cmdlets de Redes do Windows PowerShell. Para obter mais informações, consulte [Cmdlets do adaptador de rede](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps) na biblioteca do Windows Server.

## <a name="windows-update-settings"></a>Configurações do Windows Update

As configurações atuais do Windows Update são exibidas na tela padrão da Ferramenta de Configuração do Servidor. Você pode configurar o servidor para usar as atualizações automáticas ou manuais na opção de configuração **Configurações do Windows Update** no menu principal.

Quando a opção **Atualizações Automáticas** é selecionada, o sistema verifica se há atualizações e as instala todos os dias às 03h00. As alterações entram em vigor imediatamente. Quando atualizações **Manuais** forem selecionadas, o sistema não irá verificar se há atualizações automaticamente.

A qualquer momento, você pode baixar e instalar atualizações aplicáveis ​​a partir da opção **Baixar e Instalar Atualizações** no menu principal.

A opção **Somente download** verificará se há atualizações, baixará qualquer uma que esteja disponível e, em seguida, o notificará na Central de Ações quando elas estiverem prontas para instalação. Essa é a opção padrão.

## <a name="remote-desktop-settings"></a>Configurações da Área de Trabalho Remota

O status atual das configurações de área de trabalho remota é exibido na tela padrão da Ferramenta de Configuração do Servidor. Você pode definir as seguintes configurações de Área de Trabalho Remota, acessando a opção **Área de Trabalho Remota** no menu principal e seguindo as instruções na tela.

- Habilitar a Área de Trabalho Remota para Clientes que executam a Área de Trabalho Remota com Autenticação no Nível de Rede

- Habilitar a Área de Trabalho Remota para clientes que executam qualquer versão de Área de Trabalho Remota

- Desabilitar a área de trabalho remota

## <a name="date-and-time-settings"></a>Configurações de data e hora

Você pode acessar e alterar data e hora configurações, acessando o **data e hora** opção do menu principal.

## <a name="telemetry-settings"></a>Configurações de telemetria

Essa opção permite que você configure quais dados são enviados à Microsoft.

## <a name="windows-activation-settings"></a>Configurações de Ativação do Windows

Essa opção permite configurar a Ativação do Windows.

## <a name="to-enable-remote-management"></a>Para habilitar o gerenciamento remoto

Você pode habilitar vários cenários de gerenciamento remoto da opção **Configurar Gerenciamento Remoto** do menu principal:

- Gerenciamento remoto do Console de Gerenciamento Microsoft

- Windows PowerShell

- Gerenciador do Servidor  

## <a name="to-log-off-restart-or-shut-down-the-server"></a>Para fazer logoff, reiniciar ou desligar o servidor

Para fazer logoff, reiniciar ou desligar o servidor, acesse o item de menu correspondente no menu principal. Essas opções também estão disponíveis a partir de **segurança do Windows** menu que pode ser acessado de qualquer aplicativo a qualquer momento pressionando CTRL + ALT + DEL.  

## <a name="to-exit-to-the-command-line"></a>Para sair da linha de comando
  
Selecione a opção **Sair da Linha de Comando** e pressione ENTER para sair da linha de comando. Para retornar para a ferramenta de configuração do servidor, digite **sconfig. cmd**, e pressione ENTER.
