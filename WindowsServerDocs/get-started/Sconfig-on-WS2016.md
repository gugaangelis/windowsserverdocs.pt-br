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
ms.localizationpriority: high
ms.openlocfilehash: 2473b4ffae79c29ec7505616c139c03b21a4427b
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "1284342"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>Configurar uma instalação Server Core do Windows Server 2016 ou Windows Server, versão 1709, com Sconfig.cmd
> Aplicável a: Windows Server (canal semestral) e Windows Server 2016

No Windows Server 2016, Windows Server, versão 1709, você pode usar a ferramenta de Configuração do Servidor (Sconfig.cmd) para configurar e gerenciar vários aspectos comuns de instalações Server Core. Você deve ser membro do grupo Administradores para usar a ferramenta.  
  
Você pode usar o Sconfig.cmd nas instalações Server Core e Server com Experiência Desktop (somente para Windows Server 2016). 
  
## <a name="start-the-server-configuration-tool"></a>Iniciar a ferramenta de Configuração do Servidor  
  
1.  Vá para a unidade do sistema.  
  
2.  Digite `Sconfig.cmd` e pressione ENTER. A interface da ferramenta de Configuração do Servidor é aberta:  
  
 <img src="mainsconfigpage.png" style='float:left; padding:.5em;' alt="Screenshot of Sconfig.cmd user interface">  
Captura de tela da interface do usuário do Sconfig.cmd  
  
##  <a name="BKMK_Domainworkgroup"></a> Configurações de domínio/grupo de trabalho  
 As configurações de Domínio/Grupo de trabalho atuais são exibidas na tela da ferramenta de Configuração do Servidor padrão. Você pode ingressar em um domínio ou um grupo de trabalho acessando a página de configurações do **Domínio/Grupo de trabalho** no menu principal e seguindo as instruções nas páginas a seguir, fornecendo as informações necessárias.  
  
 Se um usuário de domínio não tiver sido adicionado ao grupo local de administradores, você não poderá fazer alterações no sistema, como alterar o nome do computador, usando o usuário de domínio. Para adicionar um usuário de domínio ao grupo local de administradores, reinicie o computador. Em seguida, faça logon no computador como administrador local e siga as etapas na seção [Configurações do administrador local](assetId:///3c2f8ca4-6adc-4ebd-8daf-eb0de16c2c7d#BKMK_Localadministratorsettings) mais adiante neste documento.  
  
> [!NOTE]
>  Você será solicitado a reiniciar o servidor para aplicá-las para a associação ao grupo de trabalho ou domínio. No entanto, você pode fazer alterações adicionais e reiniciar o servidor após todas as alterações para evitar a necessidade de reiniciar o servidor várias vezes. Por padrão, as máquinas virtuais em execução são salvas automaticamente antes da reinicialização do Hyper-V Server.  
  
## <a name="computer-name-settings"></a>Configurações de nome do computador  
 O nome do computador atual é exibido na tela Ferramenta de Configuração do Servidor padrão. Você pode alterar o nome do computador acessando a página de configurações "Nome do Computador" no menu principal e seguindo as instruções.  
  
> [!NOTE]
>  Você será solicitado a reiniciar o servidor para aplicá-las para a associação ao grupo de trabalho ou domínio. No entanto, você pode fazer alterações adicionais e reiniciar o servidor após todas as alterações para evitar a necessidade de reiniciar o servidor várias vezes. Por padrão, as máquinas virtuais em execução são salvas automaticamente antes da reinicialização do Hyper-V Server.  
  
##  <a name="BKMK_Localadministratorsettings"></a> Configurações de administrador local  
 Para adicionar mais usuários ao grupo local de administradores, use a opção **Adicionar Administrador Local** no menu principal. Em uma máquina de participantes de domínio, insira o usuário no seguinte formato: domínio\nome de usuário. Em uma máquina associada não de domínio (máquina de grupo de trabalho), digite apenas o nome do usuário. As alterações entram em vigor imediatamente.  
  
## <a name="network-settings"></a>Configurações de rede  
 Você pode configurar o endereço IP a ser atribuído automaticamente por um servidor DHCP ou pode atribuir um endereço IP estático manualmente. Essa opção permite que você defina as configurações do Servidor DNS também para o servidor.  
  
> [!NOTE]
>  Essas opções e muitas outras agora estão disponíveis usando os cmdlets do Windows PowerShell em rede. Para obter mais informações, consulte [Cmdlets do adaptador de rede](https://technet.microsoft.com/library/jj134956.aspx) na biblioteca do Windows Server.  
  
## <a name="windows-update-settings"></a>Configurações do Windows Update  
 As configurações atuais do Windows Update são exibidas na tela Ferramenta de Configuração do Servidor padrão. Você pode configurar o servidor para usar as atualizações Automáticas ou Manuais na opção de configuração **Configurações do Windows Update** no menu principal.  
  
 Quando **Atualizações Automáticas** forem selecionadas, o sistema procurará e instalações todos os dias às 3 da manhã. As configurações entram em vigor imediatamente. Quando atualizações **Manuais** forem selecionadas, o sistema não verificará as atualizações automaticamente.  
  
 A qualquer momento, você poderá baixar e instalar atualizações aplicáveis usando a opção **Baixar e Instalar Atualizações** no menu principal.

 A opção **Somente download** verificará se há atualizações, baixará qualquer uma que esteja disponível e, em seguida, o notificará na Central de Ações quando elas estiverem prontas para instalação. Essa é a opção padrão.  
  
## <a name="remote-desktop-settings"></a>Configurações da Área de Trabalho Remota  
 O status atual de configurações da área de trabalho remota é exibido na tela Ferramenta de Configuração do Servidor padrão. Você pode definir as seguintes configurações de Área de Trabalho Remota acessando a opção do menu principal **Área de Trabalho Remota** e seguindo as instruções na tela.  
  
-   Habilitar a Área de Trabalho Remota para Clientes que executam a Área de Trabalho Remota com Autenticação no Nível de Rede  
  
-   Habilitar a Área de Trabalho Remota para clientes que executam qualquer versão de Área de Trabalho Remota  
  
-   Desabilitar a área de trabalho remota  
  
## <a name="date-and-time-settings"></a>Configurações de data e hora  
 Você pode acessar e alterar as configurações de data e hora acessando a opção de menu principal **Data e Hora** 

## <a name="telemetry-settings"></a>Configurações de telemetria
Essa opção permite que você configure quais dados são enviados à Microsoft.

## <a name="windows-activation-settings"></a>Configurações de Ativação do Windows
Essa opção permite configurar a Ativação do Windows.
  
## <a name="to-enable-remote-management"></a>Para habilitar o gerenciamento remoto  
Você pode habilitar vários cenários de gerenciamento remoto usando a opção de menu principal **Configurar Gerenciamento Remoto**:  
  
-   Gerenciamento remoto do Console de Gerenciamento Microsoft  
-   Windows PowerShell  
-   Gerenciador do Servidor  
  
## <a name="to-log-off-restart-or-shut-down-the-server"></a>Para fazer logoff, reiniciar ou desligar o servidor  
 Para fazer logoff, reiniciar ou desligar o servidor, acesse o item de menu correspondente no menu principal. Essas opções também estão disponíveis no menu de Segurança do Windows que pode ser acessado de qualquer aplicativo a qualquer momento pressionando CTRL+ALT+DEL.  
  
## <a name="to-exit-to-the-command-line"></a>Para sair para a linha de comando  
 Selecione a opção **Saída para a Linha de Comando** e pressione ENTER para sair para a linha de comando. Para retornar à Ferramenta de Configuração do Servidor, digite **Sconfig.cmd** e pressione ENTER