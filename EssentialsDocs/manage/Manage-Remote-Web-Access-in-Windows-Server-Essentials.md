---
title: Gerenciar o acesso remoto via Web no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6f4277637ed0f721b0cae12c15086a59ac6190fc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311167"
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>Gerenciar o acesso remoto via Web no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 O Acesso via Web remoto no Windows Server Essentials ou no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, fornece uma experiência de navegador simplificada e fácil de toque para acessar aplicativos e dados de praticamente qualquer lugar que você tenha uma conexão com a Internet e usando quase qualquer dispositivo. Para usar a funcionalidade de Acesso via Web Remoto, primeiro você deve ativá-lo usando o assistente de configuração do Acesso em Qualquer Local e, em seguida, configurar seu roteador e nome de domínio.  
  
## <a name="in-this-topic"></a>Neste tópico  
  
-   [Ativar e configurar o Acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurar o roteador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurar seu nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Personalizar Acesso via Web remotos](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Solucionar problemas de Acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="turn-on-and-configure-remote-web-access"></a><a name="BKMK_1"></a>Ativar e configurar o Acesso via Web remoto  
 Os tópicos a seguir o ajudarão a ativar e configurar o Acesso via Web Remoto:  
  
-   [Visão geral de Acesso via Web remota](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Ativar Acesso via Web remotas](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [Alterar sua região](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [Gerenciar permissões de Acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [Acesso via Web remoto seguro](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [Gerenciar usuários remotos Acesso via Web e VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="remote-web-access-overview"></a><a name="BKMK_Overview"></a>Visão geral de Acesso via Web remota  
 Quando você estiver longe do seu escritório, poderá abrir um navegador da Web e acessar Acesso via Web remotas de qualquer lugar que tenha acesso à Internet. No Acesso via Web remoto, você pode:  
  
- Acessar arquivos e pastas compartilhadas no servidor.  
  
- Acessar o servidor e os computadores na rede. Isso significa que você pode acessar a área de trabalho a partir de um computador na rede como se você estivesse na frente dele no escritório.  
  
  O Acesso via Web remoto não é ativado por padrão. Quando você executar o Assistente de Configuração do Acesso em Qualquer Local, o assistente tenta configurar seu roteador e conectividade com a Internet. Após a ativação do Acesso via Web remoto, você pode configurar um nome de domínio para o servidor e personalizar o Acesso via Web remoto. Você também pode configurar o roteador novamente se você o alterar  
  
  A permissão para acessar o Acesso via Web remoto não é concedida automaticamente quando você adiciona uma nova conta de usuário. Quando você adicionar uma conta de usuário, você pode optar por permitir o acesso às pastas compartilhadas, Biblioteca de mídia, computadores, links de Página inicial e o Painel do servidor. Você também pode especificar que um usuário não tenha permissão para usar o Acesso via Web remoto.  
  
  A configuração de Acesso via Web remoto é exibida para cada conta de usuário na guia **usuários** do painel do Windows Server Essentials. Para alterar a configuração de Acesso via Web remoto, clique com o botão direito do mouse na conta de usuário e clique em **exibir as propriedades da conta**.  
  
###  <a name="turn-on-remote-web-access"></a><a name="BKMK_TurnOnRWA"></a>Ativar Acesso via Web remotas  
 Você pode ativar o Acesso via Web Remoto executando o assistente de configuração do Acesso em Qualquer Local no Painel do servidor.  
  
##### <a name="to-turn-on-remote-web-access"></a>Para ativar o Acesso via Web Remoto  
  
1.  Abra o Dashboard.  
  
2.  Clique em **Configurações** e clique na guia **Acesso em Qualquer Local**.  
  
3.  Clique em **Configurar**. O assistente de configuração do Acesso em Qualquer Local é exibido.  
  
4.  Na página **Escolher os recursos de Acesso em Qualquer Local para habilitar**, selecione a caixa de seleção **Acesso via Web remoto**.  
  
5.  Siga as instruções para concluir o assistente.  
  
###  <a name="change-your-region"></a><a name="BKMK_Region"></a>Alterar sua região  
 Você deve ser um administrador de rede para alterar a configuração de região no Windows Server Essentials.  
  
##### <a name="to-change-the-region-setting"></a>Para alterar a configuração de região  
  
1.  Abra o Painel em um computador que esteja conectado ao Windows Server Essentials.  
  
2.  Clique em **Configurações**.  
  
3.  Na guia **Geral**, clique na lista suspensa na seção **Local do país/região do servidor**.  
  
4.  Na lista suspensa, selecione a nova região e clique em **Aplicar** para aceitar a nova configuração de região.  
  
###  <a name="manage-remote-web-access-permissions"></a><a name="BKMK_ManagePerms"></a>Gerenciar permissões de Acesso via Web remoto  
 Quando você adicionar uma conta de usuário no Windows Server Essentials, o novo usuário tem permissão de usar o Acesso via Web Remoto por padrão. Se você optou por não permitir o Acesso via Web remoto de uma conta de usuário e, em seguida, descobrir que o usuário precisa usar o Acesso via Web remoto, você pode atualizar as propriedades da conta do usuário.  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>Para gerenciar permissões de Acesso via Web Remoto para uma conta de usuário  
  
1. Faça logon no Painel e em seguida, clique em **Usuários**.  
  
2. Clique na conta de usuário que você deseja gerenciar e em seguida, clique em **Exibir as propriedades de conta** no painel **Tarefas**.  
  
3. Na caixa de diálogo **Propriedades**, clique na guia **Acesso em Qualquer Local**.  
  
4. Na guia **Acesso em Qualquer Local**, marque a caixa de seleção **Permitir o Acesso via Web remoto e o acesso a aplicativos de serviços Web** para permitir que um usuário se conecte ao servidor usando o Acesso via Web remoto.  
  
5. Clique em **Aplicar**e clique em **OK**.  
  
   Para obter mais informações, consulte [gerenciar contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
###  <a name="secure-remote-web-access"></a><a name="BKMK_SecureRWA"></a>Acesso via Web remoto seguro  
 O Windows Server Essentials usa um certificado de segurança para ajudar a proteger as informações que são trocadas entre o software e um navegador da Web. Quando você instala o software Connector em seus computadores, o certificado de segurança do Windows Server Essentials é adicionado à lista de certificados confiáveis em seus computadores. A melhor maneira de os usuários acessarem o Acesso via Web Remoto quando estiverem fora do escritório é usar um computador portátil que tenha o software Connector instalado nele.  
  
> [!WARNING]
>  Os usuários que utilizam o Acesso via Web Remoto de locais públicos ou outros computadores não confiáveis devem garantir que fazem logoff do site da Internet antes de deixar o computador sem supervisão ou quando tiver concluídos suas sessões.  
  
###  <a name="manage-remote-web-access-and-vpn-users"></a><a name="BKMK_ManageRWAVPN"></a>Gerenciar usuários remotos Acesso via Web e VPN  
 Você pode usar VPN para se conectar ao Windows Server Essentials e acessar todos os seus recursos que estão armazenados no servidor. Isso é especialmente útil se você tiver um computador cliente que está configurado com contas de rede que podem ser usadas para se conectar a um servidor do Windows Server Essentials hospedado por meio de uma conexão VPN. Todas as contas de usuário recém-criadas no Windows Server Essentials hospedado devem usar VPN para fazer logon no computador cliente pela primeira vez.  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>Para definir permissões de VPN e Acesso via Web remoto para os usuários da rede  
  
1.  Abra o Dashboard.  
  
2.  Na barra de navegação, clique em **USUÁRIOS**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja conceder permissões para acessar a área de trabalho remotamente.  
  
4.  No painel **< contas de usuário\> tarefas** , clique em **Propriedades**.  
  
5.  Em **< conta de usuário\> Propriedades**, clique na guia **acesso em qualquer lugar** .  
  
6.  Na guia **Acesso em qualquer lugar**, faça o seguinte:  
  
    1.  Para permitir que um usuário se conecte ao servidor usando VPN, marque a caixa de seleção **Permitir VPN (rede privada virtual)** .  
  
    2.  Para permitir que um usuário se conecte ao servidor usando o Acesso Remoto via Web, marque a caixa de seleção **Permitir Acesso Remoto via Web e acesso a aplicativos de serviços Web**.  
  
7.  Clique em **Aplicar**e clique em **OK**.  
  
##  <a name="set-up-your-router"></a><a name="BKMK_2"></a>Configurar o roteador  
 Quando você configura seu servidor para Acesso via Web Remoto, o assistente de configuração do Acesso em Qualquer Local tenta configurar o roteador. Caso troque de roteador ou altere suas configurações, você deverá retornar ao assistente de configuração do roteador. Para obter mais informações, consulte estes tópicos:  
  
-   [Configurar o roteador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [Substituir um roteador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [Local de rede definido](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [Habilitar Serviços de Área de Trabalho Remota controles ActiveX](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="set-up-your-router"></a><a name="BKMK_SetUpRouter"></a>Configurar o roteador  
 Durante esta etapa, o Windows Server Essentials tenta configurar automaticamente seu roteador usando comandos UPnP. Para isso, seu roteador deve oferecer suporte aos padrões UPnP e a configuração de UPnP deve estar habilitada no roteador.  
  
> [!NOTE]
>  A configuração da sua rede deve seguir os requisitos de rede com suporte do Windows Server Essentials. Deve haver apenas um roteador na rede.  
  
 Caso o roteador não seja configurado pelo assistente de configuração de nome de domínio, encaminhe manualmente a porta 443. Para obter informações sobre a configuração do encaminhamento de porta no roteador, consulte [Configuração do roteador](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx).  
  
###  <a name="replace-a-router"></a><a name="BKMK_ReplaceRouter"></a>Substituir um roteador  
 Substitua o roteador de acordo com as instruções do fabricante e execute o assistente configurar o roteador para configurar o novo roteador.  
  
##### <a name="to-set-up-your-new-router"></a>Para configurar seu novo roteador  
  
1.  No Painel do Windows Server Essentials, clique em **Configurações**.  
  
2.  Clique na guia **Acesso em Qualquer Local** e em seguida, na seção **Roteador**, clique em **Configurar**. O Assistente de configuração de seu roteador é iniciado.  
  
3.  Siga as instruções do Assistente para concluir a configuração do seu novo roteador.  
  
###  <a name="network-location-defined"></a><a name="BKMK_NetworkLocation"></a>Local de rede definido  
 Um local de rede é um conjunto de configurações de rede que o Windows aplica quando você se conectar a uma rede. As configurações variam e podem ser personalizadas com base no tipo de rede que você usa. As configurações para um local de rede determinam se certos recursos (como compartilhamento de arquivos e impressora, descoberta de rede e o compartilhamento de pasta pública) são ativados ou desativados. Locais de rede são úteis quando você precisa se conectar a redes diferentes.  
  
 Por exemplo, você pode ter um laptop que você usa em casa e no trabalho. Quando estiver no escritório, você se conecta à rede do escritório. No entanto, quando você chegar em casa, você usa seu laptop para acessar e reproduzir vídeos e música armazenados no servidor doméstico. Quando você se conectar a uma nova rede e especificar o tipo de local, o Windows atribui um perfil de rede predefinido para esse tipo de local. Na próxima vez que você se conectar àquela rede, o Windows reconhece a rede e atribui automaticamente as configurações corretas. Isso adiciona uma camada de segurança para ajudar a proteger as informações no seu computador e somente os recursos de rede que você precisa para esse local são ativados.  
  
 Há quatro tipos de locais de rede:  
  
-   **Rede doméstica** Escolha essa rede para redes domésticas ou quando você conhecer e confia nas pessoas e dispositivos na rede. Computadores em uma rede doméstica podem pertencer a um grupo doméstico. Descoberta de rede está ativada para redes domésticas, o que permite que você veja outros computadores e dispositivos na rede e permite que outros usuários da rede vejam seu computador.  
  
-   **Rede corporativa** Escolha essa rede para pequenos escritórios ou outras redes de área de trabalho. Descoberta de rede, que permite que você veja outros computadores e dispositivos em uma rede e permite que outros usuários da rede vejam seu computador, é ativada por padrão, mas você não pode criar ou ingressar em um grupo doméstico.  
  
-   **Rede pública** Escolha essa rede para locais públicos (como cafés ou aeroportos). Esse local foi projetado para que seu computador não seja visível para outros computadores e para ajudar a proteger o computador contra softwares maliciosos da Internet. Grupo doméstico não está disponível em redes públicas e a descoberta de rede está desativada. Você também deve escolher essa opção se estiver conectado diretamente à Internet sem usar um roteador ou se você tiver uma conexão de banda larga móvel.  
  
-   **Domínio** Escolha essa rede para domínios, como aqueles em locais de trabalho da empresa. Esse tipo de local de rede é controlado pelo seu administrador de rede e não pode ser selecionado ou alterado.  
  
###  <a name="enable-remote-desktop-services-activex-controls"></a><a name="BKMK_ActiveX"></a>Habilitar Serviços de Área de Trabalho Remota controles ActiveX  
 O Serviços de Área de Trabalho Remota controles ActiveX permite que você acesse seu computador doméstico ou comercial, por meio da Internet, de outro computador usando o Acesso via Web remoto.  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>Para habilitar os controles ActiveX de serviços de área de trabalho remota  
  
1.  No Internet Explorer, clique em **Ferramentas** e clique em **Opções da Internet**.  
  
2.  Na guia **Segurança**, clique em **Nível personalizado**.  
  
3.  Na seção **Controles ActiveX e plug-ins**, faça o seguinte:  
  
    1.  Em **Baixar Controles ActiveX assinados**, clique em **Prompt**.  
  
    2.  Em **Executar Controles ActiveX e plug-ins**, clique em **Habilitar**.  
  
4.  Clique em **OK** duas vezes para aceitar as alterações e fechar a caixa de diálogo.  
  
##  <a name="set-up-your-domain-name"></a><a name="BKMK_3"></a>Configurar seu nome de domínio  
 Depois que o Acesso via Web Remoto é ativado, você pode configurar um nome de domínio para o servidor que executa o Windows Server Essentials. Essa é uma etapa necessária para usar o Acesso via Web Remoto a partir de um computador remoto. Para obter mais informações, consulte estes tópicos:  
  
-   [Visão geral de nomes de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [Entender os nomes de domínio personalizados da Microsoft](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [Usar um nome de domínio novo ou existente](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [Configurar um nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [Escolher um provedor de serviços de nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [Escolher um nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [Escolher um prefixo de nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [Escolher uma extensão de nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [Atualizar ou atualizar seu serviço de nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [Exportar ou importar seu certificado no servidor](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [Configurar um nome de domínio manualmente](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [Localizar seu provedor de serviços de nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="domain-names-overview"></a><a name="BKMK_DNOverview"></a>Visão geral de nomes de domínio  
 Um nome de domínio identifica exclusivamente seu servidor na Internet. Nomes de domínio consistem em pelo menos duas partes: um nome de domínio de nível superior (TLD) e um nome de domínio de segundo nível. Por exemplo, em contoso.com, com é o TLD e contoso é o nome de domínio de segundo nível.  
  
 Enquanto você estiver fora do escritório, você pode usar seu nome de domínio para acessar arquivos compartilhados no servidor ou computadores na rede. Você também pode gerenciar seu servidor quando estiver ausente. Por exemplo, você deve registrar contoso.com em seu servidor. Quando você está fora do escritório, você pode abrir um navegador da Web em seu laptop e digitar **contoso.com** na caixa de texto de endereço para se conectar à instância do Aacesso via Web Remoto que você configurou no Windows Server Essentials.  
  
###  <a name="understand-microsoft-personalized-domain-names"></a><a name="BKMK_PersonalizedNames"></a>Entender os nomes de domínio personalizados da Microsoft  
 Um nome de domínio personalizado da Microsoft inclui os seguintes recursos:  
  
- Um nome de domínio personalizado para Acesso via Web remoto (por exemplo, *yourhostname*. remotewebaccess.com). Seu nome do domínio é associado com seu endereço IP público.  
  
- Um serviço de protocolo de atualização dinâmica de DNS para que o Acesso via Web remoto usando seu nome de domínio não seja interrompido se seu endereço IP público for alterado. Normalmente, os provedores de serviços de Internet (ISPs) para as conexões de banda larga de sua organização fornecem endereços IP públicos dinâmicos que podem ser alterados.  
  
- Um certificado confiável associado com o nome de domínio.  
  
  Para integrar um nome de domínio personalizado da Microsoft com seu servidor, você precisa de uma conta da Microsoft (anteriormente conhecida como Windows Live ID). Se você não tiver uma conta da Microsoft, pode se registrar em uma no site [Microsoft Hotmail](https://login.live.com/) .  
  
> [!IMPORTANT]
>  O Windows Live permite caracteres especiais em sua senha da conta da Microsoft que não têm suporte no servidor. Se você usar um domínio personalizado da Microsoft, certifique-se de que sua senha da conta da Microsoft contenha apenas caracteres com suporte no servidor. O servidor não tem suporte ao uso dos caracteres $, /, ‘ e %.  
  
###  <a name="use-a-new-or-existing-domain-name"></a><a name="BKMK_UseNewName"></a>Usar um nome de domínio novo ou existente  
 Para configurar automaticamente seu nome de domínio em um servidor que executa o Windows Server Essentials, você deve usar um provedor de serviços de nome de domínio listado no Assistente de configuração de seu nome de domínio. Você pode optar por obter um novo nome de domínio ou usar um nome de domínio existente. Siga um destes procedimentos:  
  
-   Se você deseja obter um novo nome de domínio dos provedores de serviço de nome de domínio listados no assistente, clique em **Desejo configurar um novo nome de domínio**.  
  
-   Se você tiver um nome de domínio existente, comprado de um dos provedores de serviços de nome de domínio com suporte, você pode usar o Assistente de configuração de nome de domínio para configurar o nome de domínio para o seu servidor. Clique em **Desejo usar um nome de domínio que já possuo** e digite o nome do domínio na caixa de texto **Definir seu nome de domínio**. Você deve fornecer o nome de usuário e a senha que você usou para adquirir o nome de domínio.  
  
-   Se você tiver um nome de domínio existente, comprado de um provedor de serviços de nome de domínio que não tem suporte no Windows Server Essentials e você deseja usar o Assistente de configuração de nome de domínio para configurar o nome de domínio para o seu servidor, você pode transferir o nome de domínio para um dos provedores de serviços de nome de domínio listados no assistente. Clique em **desejo usar um nome de domínio que eu já possuo**, digite o nome de domínio na caixa de texto **nome de domínio** e siga as instruções no site do provedor de serviço de nome de domínio para transferir o nome de domínio.  
  
###  <a name="set-up-a-domain-name"></a><a name="BKMK_SetUpName"></a>Configurar um nome de domínio  
 Quando você ativa o acesso via Web Remoto, você pode optar por configurar o nome de domínio da Internet do servidor.  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>Para configurar ou gerenciar um nome de domínio da Internet  
  
1.  Abra o Dashboard.  
  
2.  Clique em **Configurações do servidor** e clique na guia **Acesso em Qualquer Local**.  
  
3.  Na seção **Nome de domínio**, clique em **Configurar**.  
  
4.  Siga as instruções para concluir o assistente. Se você ainda não tem um nome de domínio e um certificado, o assistente ajuda você a encontrar um provedor de nomes de domínio para adquirir um nome de domínio e um certificado ou você pode obter um nome de domínio personalizado da Microsoft.  
  
###  <a name="choose-a-domain-name-service-provider"></a><a name="BKMK_ChooseProvider"></a>Escolher um provedor de serviços de nome de domínio  
 Você deve escolher um provedor de serviços de nome de domínio que suporte a extensão de nome de domínio que você deseja usar. O assistente configurar seu nome de domínio inclui uma lista de provedores qualificados que você pode usar com um link para cada site do provedor. Clique no link **mais informações** ao lado de cada nome de provedor para obter informações sobre os serviços e os preços que são oferecidos pelo provedor.  
  
> [!NOTE]
>  Alguns provedores de serviços de nome de domínio servem amplas regiões internacionais e outros atendem mercados menores. Por isso, alguns provedores podem não oferecer um site que traduzido em seu idioma preferencial.  
  
 Quando você compra seu nome de domínio, você também pode considerar adquirir o serviço de protocolo de atualização dinâmica do sistema de nome de domínio (DNS) de seu provedor de serviços de nome de domínio. O Protocolo de atualização dinâmica do DNS é um serviço que permite que qualquer pessoa na Internet acesse recursos em uma rede local quando o endereço IP da rede está em constante mudança. Ou você pode adquirir um endereço IP estático de seu provedor de serviços de Internet (ISP) para assegurar que seu endereço IP não seja alterado.  
  
###  <a name="choose-a-domain-name"></a><a name="BKMK_ChooseDomainName"></a>Escolher um nome de domínio  
 Escolher um nome que identifique exclusivamente o seu servidor de negócios. Por exemplo, se o nome da empresa for Contoso Ltd, você poderá escolher Contoso para identificar exclusivamente o seu servidor doméstico ou corporativo na Internet. Se o nome de domínio não estiver disponível, tente outra variação desse nome ou talvez algo completamente diferente.  
  
 O nome digitado pode conter o seguinte:  
  
-   máximo de 63 caracteres  
  
-   Letras (Inglês ou caracteres localizados), números ou hifens (-). O nome deve começar e terminar com uma letra ou um número.  
  
    > [!NOTE]
    >  Nomes de domínio não diferenciam maiúsculas de minúsculas.  
  
###  <a name="choose-a-domain-name-prefix"></a><a name="BKMK_Prefixes"></a>Escolher um prefixo de nome de domínio  
 Um nome de domínio consiste em rótulos hierárquicos.  
  
 **A extensão do domínio de primeiro nível** é o rótulo mais à direita no nome do domínio. Por exemplo, na www\.contoso.com, com é a extensão de nome de domínio de nível superior.  
  
 **O nome de domínio de segundo nível** é o rótulo próximo à extensão de nome de domínio de primeiro nível. O nome de domínio de segundo nível geralmente é criado com base no nome da empresa, produtos ou serviços. Por exemplo, na www\.contoso.com, contoso é o nome de domínio de segundo nível e foi escolhido para o nome da empresa Contoso Pharmaceuticals. O domínio de segundo nível às vezes também é chamado de nome de host, que tem um endereço IP associado a ele.  
  
 **O prefixo de nome de domínio** identifica um subdomínio. O nome de subdomínio pode ser usado para identificar serviços, dispositivos ou regiões. Por exemplo, a Contoso Pharmaceuticals deseja permitir que os usuários remotos façam logon no Acesso via Web Remoto, mas não quer que o site esteja disponível para o público, então, eles criam um subdomínio que permite que apenas os usuários com permissões apropriadas acessem o site. A Contoso Pharmaceuticals configura remote.contoso.com como o subdomínio e remoto é o prefixo de nome de domínio.  
  
> [!TIP]
>  É recomendável que você use o padrão **Remoto** como o prefixo de nome de domínio.  
  
###  <a name="choose-a-domain-name-extension"></a><a name="BKMK_Extension"></a>Escolher uma extensão de nome de domínio  
 Quando você escolher um nome de domínio para o site da Internet, você também precisa especificar a extensão de nome de domínio que você deseja usar. A extensão é identificada por letras após o ponto final de qualquer nome de domínio. (O termo formal para a extensão é o domínio de nível superior ou TLD.)  
  
 Há dois tipos principais de extensões de domínio que você pode usar: genérico e código do país.  
  
#### <a name="generic-top-level-domains"></a>Domínios de nível superior genéricos  
 Extensões de domínio genérico têm três ou mais letras e normalmente são usadas por determinados tipos de organizações.  
  
 **Exemplos de domínios de nível superior genéricos**  
  
|Extensão do domínio|Descrição|  
|----------------------|-----------------|  
|.com|Normalmente usado por organizações comerciais, mas pode ser usado por qualquer pessoa.|  
|.net|Projetado para empresas que oferecem serviços de infraestrutura de rede.|  
|.org|Originalmente usados por organizações sem fins lucrativos e outros negócios que não se encaixam em outra categoria de domínio de primeiro nível genérico. Pode ser usado por qualquer pessoa.|  
|.edu|Restrito para uso por organizações de ensino.|  
  
#### <a name="country-code-top-level-domains"></a>Domínios de nível superior de código do país  
 Essas extensões de domínio têm duas letras. Eles são projetados para serem usados por organizações no país ou região associada com esse código. Alguns domínios de nível superior de código do país são restritos para uso por cidadãos desse país ou região. Outros estão disponíveis para uso por qualquer pessoa.  
  
 **Exemplos de domínios de nível superior de código de país**  
  
|Extensão do domínio|Descrição|  
|----------------------|-----------------|  
|.ca|Para uso por sites no Canadá|  
|.cn|Para uso por sites na China|  
|.de|Para uso por sites na Alemanha|  
|.co.uk|Para uso por sites no Reino Unido|  
  
 Para exibir a lista completa de domínios de nível superior, consulte o [site da Internet Assigned Numbers Authority](https://go.microsoft.com/fwlink/?LinkId=117438).  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>Se uma extensão de domínio não estiver disponível para seleção no Assistente de configuração de nome de domínio  
 Quando você executar o Assistente de configuração de nome de domínio, o assistente analisa as informações do seu sistema para determinar seu país ou região. O assistente exibe apenas as extensões de domínio que têm suporte dos provedores participantes em sua área. Se a extensão do domínio que você deseja não aparecer na lista, você deve escolher uma extensão de domínio diferente para continuar. Selecione uma extensão da lista que o assistente mostrar.  
  
###  <a name="update-or-upgrade-your-domain-name-service"></a><a name="BKMK_UpdateService"></a>Atualizar ou atualizar seu serviço de nome de domínio  
 Talvez seja necessário atualizar o serviço de nomes de domínio se você tiver adquirido um nome de domínio, mas não tiver adquirido um certificado. Você deve ter um certificado para o nome de domínio de seu provedor de serviços de nome de domínio.  
  
> [!NOTE]
>  Trabalhe com seu provedor de serviços de nome de domínio para determinar o tipo de certificado que você precisa. O certificado pode ser um dos certificados de baixo custo oferecidos. No entanto, você deve examinar a documentação e os recursos de certificados de segurança de nível superior para determinar se eles atender melhor as necessidades da sua empresa.  
  
###  <a name="export-or-import-your-certificate-on-your-server"></a><a name="BKMK_ExportCert"></a>Exportar ou importar seu certificado no servidor  
 Se você deseja criar uma cópia de backup de um certificado ou usá-lo em outro servidor, você deve exportar o certificado. Para informações sobre como exportar certificados, consulte [Exportar um certificado](https://go.microsoft.com/fwlink/p/?LinkId=214362).  
  
###  <a name="set-up-a-domain-name-manually"></a><a name="BKMK_SetNameManually"></a>Configurar um nome de domínio manualmente  
 Caso selecione esta opção, o servidor não irá monitorar ou manter o nome do domínio, nem o alertará caso haja um problema de configuração. Use também esta opção se:  
  
- Não houver provedores de nome de domínio de parceiros listados para seu país ou região.  
  
- Os provedores de domínio de parceiros listados não oferecerem suporte à sua extensão de nome de domínio.  
  
- Você tem um nome de domínio existente de um provedor de nomes de domínio que não é atualmente um parceiro e você não deseja transferir esse nome de domínio para um provedor de nomes de domínio com suporte do Windows Server Essentials.  
  
- O assistente não listar a extensão de nome de domínio que você deseja usar, mas a extensão estiver disponível para um provedor de nomes de domínio que não é um parceiro.  
  
  Se você optar por configurar o nome de domínio manualmente, trabalhe com seu provedor de serviço de nome de domínio para criar um registro A para seu domínio.  
  
##### <a name="to-create-an-a-record"></a>Para criar um registro A  
  
1.  Escolha um nome de host, como remoto. Esse é o prefixo do nome de domínio. O prefixo do nome de domínio mais seu nome de domínio definirá a URL para abrir sua página de logon Acesso via Web remota; por exemplo, **http://remote.contoso.com** .  
  
2.  Em seu painel de configuração de provedores de serviços de nome de domínio (geralmente em sua página da Web), crie o registro a para o nome do host que você escolheu na etapa 1. Verifique se o endereço IP que você especificou no registro a é o endereço IP no lado da WAN do seu roteador (o lado voltado para a Internet). Consulte a documentação do roteador para encontrar o endereço IP da WAN.  
  
3.  Recomenda-se entrar em contato com seu ISP (provedor de serviços de Internet) a fim de adquirir um endereço IP estático para sua rede. Isso garante que o endereço IP não mude e que a entrada DNS não fique desatualizada.  
  
     Caso não tenha a opção de obtenção de um endereço IP estático de seu ISP, considere adquirir também o serviço de protocolo de atualização dinâmica de DNS (Sistema de Nomes de Domínio) de seu provedor de serviços de nome de domínio ou outro provedor de serviços. O protocolo de atualização dinâmica de DNS é um serviço que mantém o endereço IP da WAN de sua rede atualizado, de modo que esse endereço IP possa ser resolvido para o nome do domínio mesmo que o endereço IP mude.  
  
4.  Importe um certificado de confiança quando o assistente solicitar. Caso não tenha um certificado de confiança, obtenha um com um dos provedores de nomes de serviço com suporte listados no assistente ou adquira um do provedor de confiança de sua escolha. Para obter mais informações sobre um certificado de confiança, entre em contato com seu provedor de nomes de domínio.  
  
###  <a name="find-your-domain-name-service-provider"></a><a name="BKMK_Find"></a>Localizar seu provedor de serviços de nome de domínio  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>Para localizar o provedor de serviços de nome de domínio para seu nome de domínio  
  
1. Abra um navegador da Web e digite <strong>www.internic.com</strong> na barra de endereços para ir para página inicial da Internic.  
  
2. Na página inicial da Internic, clique em **Whois**.  
  
3. Na caixa **Whois**, digite o nome do seu domínio (por exemplo, contoso.com).  
  
4. Clique na opção **Domínio** e em seguida, clique em **Enviar**.  
  
5. Nos resultados da pesquisa, o nome do seu provedor de serviços de nome de domínio está listado sob **Registrar**.  
  
##  <a name="customize-remote-web-access"></a><a name="BKMK_4"></a>Personalizar Acesso via Web remotos  
 Você pode personalizar o site de Acesso via Web Remoto adicionando um logotipo pessoal ou uma imagem de fundo. Você também pode adicionar links na Home page, de modo que essas informações fiquem disponíveis para todos os usuários. Para obter mais informações, consulte estes tópicos:  
  
-   [Personalizar Acesso via Web remotos](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [Personalizar imagens para planos de fundo e logotipos](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [Reparar Acesso via Web remotos](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="customize-remote-web-access"></a><a name="BKMK_CustomizeRWA"></a>Personalizar Acesso via Web remotos  
 Você pode personalizar o Acesso via Web Remoto alterando o título do site, alterando a imagem de plano de fundo e logotipo e adicionando links para outros sites na página inicial.  
  
##### <a name="to-customize-remote-web-access"></a>Personalizaro  Acesso via Web Remoto  
  
1.  Abra o Dashboard.  
  
2.  Clique em **Configurações** e clique na guia **Acesso em Qualquer Local**.  
  
3.  Na seção **Configurações do site**, clique em **Personalizar**.  
  
4.  Quando você concluir a personalização do Acesso via Web Remoto, clique em **OK**. Teste as alterações no Acesso via Web Remoto.  
  
###  <a name="customize-images-for-backgrounds-and-logos"></a><a name="BKMK_CustomizeImages"></a>Personalizar imagens para planos de fundo e logotipos  
 Esta seção fornece informações sobre as imagens que você pode usar para personalizar o Acesso via Web Remoto.  
  
#### <a name="image-size"></a>Tamanho da imagem  
 **Imagens de logotipo**  
  
 É recomendável que você use as imagens de logotipo com 32 x 32 pixels. Imagens maiores são reduzidas para 32 x 32 e imagens menores são aumentadas para 32 x 32, o que pode distorcer a imagem.  
  
 **Imagens de plano de fundo**  
  
 Embora não haja nenhum limite de tamanho para imagens de plano de fundo, para obter melhores resultados, é recomendável que você use imagens de aproximadamente 800 x 500 pixels. A imagem de plano de fundo é colocada no centro (horizontal e vertical) da página de logon. Para ajudar a facilitar a leitura do texto na página de logon, o centro da imagem de plano de fundo deve ser de cor clara.  
  
#### <a name="image-file-types"></a>Tipos de arquivo de imagem  
 Os seguintes tipos de arquivo de imagem podem ser usados para substituir o logotipo de plano de fundo e site padrão:  
  
-   Bitmap (*. bmp, \*. dib, \*. RLE)  
  
-   GIF (*.gif)  
  
-   PNG (*.png)  
  
-   JPG (*.jpg)  
  
###  <a name="repair-remote-web-access"></a><a name="BKMK_RepairRWA"></a>Reparar Acesso via Web remotos  
 O Assistente de reparo ajuda a detectar e resolver problemas com seu roteador ou o nome de domínio. Há duas maneiras de descobrir problemas com o Acesso via Web Remoto:  
  
-   Nas configurações do servidor do Painel, na guia Acesso em Qualquer Local, um ícone é exibido com um X vermelho junto com uma descrição do problema.  
  
-   Um alerta no Visualizador de Alertas.  
  
> [!NOTE]
>  O Assistente de reparo não está disponível até que você ative o Acesso via Web Remoto. Para obter informações sobre como ativar o Acesso via Web Remoto, veja [Ativar o Acesso via Web Remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
##### <a name="to-repair-remote-web-access"></a>Reparar o Acesso via Web Remoto  
  
1.  Faça logon no Painel.  
  
2.  Clique em **Configurações** e clique na guia **Acesso em Qualquer Local**.  
  
3.  Clique em **Reparar**. O Assistente **Reparo do Acesso via Web Remoto** é iniciado.  
  
4.  Clique em **Avançar**. O assistente analisa o Acesso via Web Remoto, identifica o problema e tenta repará-lo.  
  
5.  Se você receber um alerta quando o assistente for concluído, você pode clicar em **Tentar novamente** para tentar reparar o problema novamente. Se você continuar a receber um alerta, verifique o alerta para obter informações adicionais sobre o problema e as etapas de solução de problemas.  
  
##  <a name="troubleshoot-remote-web-access"></a><a name="BKMK_5"></a>Solucionar problemas de Acesso via Web remoto  
  
-   [Solucionar problemas de conectividade de Acesso via Web remota](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [Solucionar problemas do firewall](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [Solucionar problemas de acesso em qualquer local](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>Consulte também  
  
-   [Opções de área de trabalho remota](Remote-desktop-options.md)  
  
-   [Usar Acesso via Web remotos](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar acesso em qualquer local](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
