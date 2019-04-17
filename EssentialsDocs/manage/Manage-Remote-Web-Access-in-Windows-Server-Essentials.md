---
title: Gerenciar o acesso via Web remoto no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: de01b2fd2395377b6e7b3349b9862eb0e51a59b8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>Gerenciar o acesso via Web remoto no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Acesso via Web remoto no Windows Server Essentials ou no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, oferece uma experiência de navegador simplificada e sensível ao toque para acessar aplicativos e dados de praticamente qualquer lugar que você tenha uma conexão de Internet e usando praticamente qualquer dispositivo. Para usar a funcionalidade de acesso via Web remoto, você deve primeiro ativá-lo usando o Assistente de configuração de qualquer lugar acesso e, em seguida, configurar o seu roteador e o nome de domínio.  
  
## <a name="in-this-topic"></a>Neste tópico  
  
-   [Ativar e configurar o acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurar o roteador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurar o seu nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Personalizar o acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Solucionar problemas de acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="BKMK_1"></a>Ativar e configurar o acesso via Web remoto  
 Os tópicos a seguir ajudarão você a ativar e configurar o acesso via Web remoto:  
  
-   [Visão geral de acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Ativar o acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [Alterar sua região](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [Gerenciar permissões de acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [Proteger o acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [Gerenciar usuários de acesso via Web remoto e VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="BKMK_Overview"></a>Visão geral de acesso via Web remoto  
 Quando você estiver fora do escritório, você pode abrir um navegador da web e acessar o acesso via Web remoto em qualquer lugar com acesso à Internet. Acesso via Web remoto, você pode:  
  
-   Acesse arquivos e pastas no servidor compartilhados.  
  
-   Acesse o servidor e computadores na rede. Isso significa que você pode acessar a área de trabalho de um computador em rede, como se você estivesse sentado em frente a ele no seu escritório.  
  
  Acesso via Web remoto não está ativado por padrão. Quando você executa a configurar o Assistente de acesso em qualquer lugar, o assistente tenta configurar o seu roteador e conectividade com a Internet. Depois que o acesso via Web remoto está ativado, você pode configurar um nome de domínio do seu servidor e personalizar o acesso via Web remoto. Você pode também configurar o roteador novamente se você alterar seu roteador.  
  
 Permissão para acessar o acesso via Web remoto não é concedido automaticamente quando você adiciona uma nova conta de usuário. Quando você adiciona uma conta de usuário, você pode optar por permitir o acesso a pastas compartilhadas, a biblioteca de mídia, computadores, links da página inicial e o painel do servidor. Você também pode especificar que um usuário não poderá usar o acesso via Web remoto.  
  
 A configuração de acesso via Web remoto é exibida para cada conta de usuário no **usuários** guia do Windows Server Essentials Dashboard. Para alterar a configuração de acesso via Web remoto, clique com botão direito a conta de usuário e, em seguida, clique em **exibir as propriedades da conta**.  
  
###  <a name="BKMK_TurnOnRWA"></a>Ativar o acesso via Web remoto  
 Você pode ativar o acesso via Web remoto, executando a configurar o Assistente de acesso em qualquer lugar do servidor de painel.  
  
##### <a name="to-turn-on-remote-web-access"></a>Para ativar o acesso via Web remoto  
  
1.  Abra o painel.  
  
2.  Clique em **configurações**e, em seguida, clique no **acesso em qualquer local** guia.  
  
3.  Clique em **configurar**. Assistente de configuração de qualquer lugar acesso aparece.  
  
4.  Sobre o **escolher qualquer lugar acessar os recursos para permitir que** página, selecione o **acesso via Web remoto** caixa de seleção.  
  
5.  Siga as instruções para concluir o assistente.  
  
###  <a name="BKMK_Region"></a>Alterar sua região  
 Você deve ser um administrador de rede para alterar a configuração de região no Windows Server Essentials.  
  
##### <a name="to-change-the-region-setting"></a>Para alterar a configuração de região  
  
1.  Em um computador que esteja conectado ao Windows Server Essentials, abra o painel.  
  
2.  Clique em **configurações**.  
  
3.  No **geral** guia, clique na lista suspensa no **local do país/região do servidor** seção.  
  
4.  Na lista suspensa, selecione nova região e, em seguida, clique em **aplicar** para aceitar a nova configuração de região.  
  
###  <a name="BKMK_ManagePerms"></a>Gerenciar permissões de acesso via Web remoto  
 Quando você adiciona uma conta de usuário no Windows Server Essentials, o novo usuário é permitido por padrão para usar o acesso via Web remoto. Se você optar por não permitir o acesso via Web remoto para uma conta de usuário e, em seguida, saiba que o usuário precisa usar o acesso via Web remoto, você pode atualizar as propriedades de s da conta de usuário.  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>Para gerenciar permissões de acesso via Web remoto para uma conta de usuário  
  
1.  Fazer logon no painel e clique em **usuários**.  
  
2.  Clique na conta de usuário que você deseja gerenciar e, em seguida, clique em **exibir as propriedades da conta** no **tarefas** painel.  
  
3.  No **propriedades** caixa de diálogo, clique no **acesso em qualquer local** guia.  
  
4.  Sobre o **acesso em qualquer local**, selecione o **permitir acesso remoto via Web e acesso a aplicativos web services** caixa de seleção Permitir que um usuário se conectar ao servidor usando o acesso via Web remoto.  
  
5.  Clique em **aplicar**e clique em **Okey**.  
  
 Para obter mais informações, consulte [gerenciar contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_SecureRWA"></a>Proteger o acesso via Web remoto  
 Windows Server Essentials usa um certificado de segurança para ajudar a proteger as informações que são trocadas entre o software e um navegador da web. Quando você instala o software do conector em seus computadores, o certificado de segurança do Windows Server Essentials é adicionado à lista de certificados confiáveis em seus computadores. A melhor maneira dos usuários acessem o acesso via Web remoto quando estiverem fora do escritório é usar um computador portátil que tem o software de conector instalado nele.  
  
> [!WARNING]
>  Os usuários que usam o acesso via Web remoto em locais públicos ou outros computadores não confiáveis devem garantir que o site fazem logoff antes de sair do computador autônomo ou quando eles estiverem terminar sua sessão.  
  
###  <a name="BKMK_ManageRWAVPN"></a>Gerenciar usuários de acesso via Web remoto e VPN  
 Você pode usar VPN para se conectar ao Windows Server Essentials e acessar todos os recursos que são armazenados no servidor. Isso é especialmente útil se você tiver um computador cliente que é configurado com contas de rede que podem ser usadas para se conectar a um servidor Windows Server Essentials hospedado por meio de uma conexão VPN. Todas as contas de usuário recém criada no servidor Windows Server Essentials hospedado devem usar VPN para fazer logon no computador cliente pela primeira vez.  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>Para definir as permissões de VPN e acesso via Web remoto para usuários de rede  
  
1.  Abra o painel.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja conceder permissões para acessar a área de trabalho remota.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **propriedades**.  
  
5.  Em **< usuário Account\ > propriedades**, clique no **acesso em qualquer local** guia.  
  
6.  Sobre o **acesso em qualquer local** guia, faça o seguinte:  
  
    1.  Para permitir que um usuário se conectar ao servidor usando VPN, selecione o **permitir rede Virtual privada (VPN)** caixa de seleção.  
  
    2.  Para permitir que um usuário se conectar ao servidor usando o acesso via Web remoto, selecione o **permitir acesso remoto via Web e acesso a aplicativos web services** caixa de seleção.  
  
7.  Clique em **aplicar**e clique em **Okey**.  
  
##  <a name="BKMK_2"></a>Configurar o roteador  
 Quando você configurar seu servidor para acesso via Web remoto, o Assistente de configuração de qualquer lugar acesso tenta configurar o roteador. Se você alterar roteadores ou alterar as configurações do roteador, você deve executar novamente o seu roteador Assistente de configuração. Para obter mais informações, consulte os seguintes tópicos:  
  
-   [Configurar o roteador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [Substituir um roteador](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [Local de rede definido](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [Habilitar controles ActiveX de serviços de área de trabalho remota](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="BKMK_SetUpRouter"></a>Configurar o roteador  
 Durante esta etapa, o Windows Server Essentials tenta configurar automaticamente o roteador usando comandos de UPnP. Para fazer isso, o roteador deve dar suporte a UPnP padrões e a configuração UPnP deve estar habilitada no seu roteador.  
  
> [!NOTE]
>  A configuração de rede deve seguir os requisitos de rede com suporte do Windows Server Essentials. Deve haver apenas um roteador em sua rede.  
  
 Se o roteador não é configurado pelo Assistente de configuração de seu domínio nome, você deve encaminhar porta 443 manualmente. Para obter informações sobre como configurar o encaminhamento de porta no roteador, consulte [configuração de roteador](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx).  
  
###  <a name="BKMK_ReplaceRouter"></a>Substituir um roteador  
 Substitua o roteador de acordo com as instruções do fabricante s e, em seguida, execute o Assistente de configuração de seu roteador para configurar o roteador novo.  
  
##### <a name="to-set-up-your-new-router"></a>Para configurar seu novo roteador  
  
1.  No painel Windows Server Essentials, clique em **configurações**.  
  
2.  Clique no **acesso em qualquer local** guia e, em seguida, no **roteador** seção, clique em **configurar**. Inicia o Assistente de configuração de seu roteador.  
  
3.  Siga as instruções no Assistente para concluir a configuração seu novo roteador.  
  
###  <a name="BKMK_NetworkLocation"></a>Local de rede definido  
 Um local de rede é uma coleção de configurações de rede que Windows aplica quando você se conectar a uma rede. As configurações variam e podem ser personalizadas com base no tipo de rede que você usa. As configurações para um local de rede determinam se determinados recursos (como arquivos e compartilhamento de impressora, descoberta de rede e compartilhamento de pasta pública) estão ativados ou desativado. Locais de rede são úteis quando você precisa se conectar a redes diferentes.  
  
 Por exemplo, você pode tiver um notebook que você usa em casa e no trabalho. Quando você estiver no escritório, você se conectar à rede office. No entanto, assim que você entra inicial, você usa seu notebook para acessar e reproduzir vídeos e músicas que é armazenada no servidor doméstico. Quando você se conectar a uma nova rede e especifica o tipo de local, o Windows atribui um perfil de rede predefinido para esse tipo de local. Na próxima vez que você se conectar à rede, o Windows reconhece a rede e atribui automaticamente as configurações corretas. Isso adiciona uma camada de segurança para ajudar a proteger as informações em seu computador, e apenas os recursos de rede que você precisa para esse local são ativados.  
  
 Há quatro tipos de locais de rede:  
  
-   **Rede doméstica** escolher essa rede para redes domésticas ou quando você conhece e confia as pessoas e os dispositivos na rede. Computadores em uma rede doméstica podem pertencer a um grupo doméstico. Descoberta de rede está ativada para redes domésticas, que permite que você veja outros computadores e dispositivos na rede e permite que outros usuários da rede vejam seu computador.  
  
-   **Rede de trabalho** escolher essa rede de pequena empresa ou outras redes de área de trabalho. Descoberta de rede, que permite que você veja outros computadores e dispositivos em uma rede e permite que outros usuários da rede vejam seu computador, é ativada por padrão, mas você não pode criar ou ingressar em um grupo doméstico.  
  
-   **Rede pública** escolher essa rede para locais públicos (como cafés ou aeroportos). Esse local destina-se para manter o seu computador fique visível para outros computadores e para ajudar a proteger seu computador contra software mal-intencionado da Internet. Grupo doméstico não está disponível em redes públicas e descoberta de rede está desativada. Você também deve escolher essa opção se você estiver conectado diretamente à Internet sem usar um roteador, ou se você tiver uma conexão de banda larga móvel.  
  
-   **Domínio** escolher essa rede para domínios como aqueles em locais de trabalho da empresa. Esse tipo de local de rede é controlado pelo seu administrador de rede, e não pode ser selecionado ou alterado.  
  
###  <a name="BKMK_ActiveX"></a>Habilitar controles ActiveX de serviços de área de trabalho remota  
 Os controles ActiveX de serviços de área de trabalho remota permite que você acesse seu computador residencial ou comercial, via Internet, de outro computador usando o acesso via Web remoto.  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>Para habilitar os controles ActiveX de serviços de área de trabalho remota  
  
1.  No Internet Explorer, clique em **ferramentas**e clique em **opções da Internet**.  
  
2.  Sobre o **segurança**, clique em **nível personalizado**.  
  
3.  No **ActiveX plug-ins e controles** seção, faça o seguinte:  
  
    1.  Em **carregar controles ActiveX assinados**, clique em **Prompt**.  
  
    2.  Em **executar controles ActiveX e plug-ins**, clique em **habilitar**.  
  
4.  Clique em **Okey** duas vezes para aceitar as alterações e fechar a caixa de diálogo.  
  
##  <a name="BKMK_3"></a>Configurar o seu nome de domínio  
 Depois que o acesso via Web remoto está ativado, você pode configurar um nome de domínio para o servidor que está executando o Windows Server Essentials. Isso é uma etapa necessária se você pretende usar o acesso via Web remoto de um computador remoto. Para obter mais informações, consulte os seguintes tópicos:  
  
-   [Visão geral de nomes de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [Entender os nomes de domínio Microsoft personalizada](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [Use um nome de domínio novos ou existentes](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [Configurar um nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [Escolher um provedor de serviço de nomes de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [Escolha um nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [Escolha um prefixo de nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [Escolha uma extensão de nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [Atualizar ou atualize o seu serviço de nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [Exportar ou importar o certificado no servidor](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [Configurar um nome de domínio manualmente](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [Encontre seu provedor de serviços de nome de domínio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="BKMK_DNOverview"></a>Visão geral de nomes de domínio  
 Um nome de domínio identifica exclusivamente o seu servidor na Internet. Nomes de domínio consistem em pelo menos duas partes: um nome de domínio de nível superior (TLD) e um segundo nome de domínio de nível. Por exemplo, em contoso.com, com é o TLD e contoso é o nome de domínio de nível segundo.  
  
 Enquanto você estiver fora do escritório, você pode usar o nome de domínio para acessar arquivos compartilhados no servidor ou computadores na rede. Você também pode gerenciar seu servidor quando estiver ausente. Por exemplo, você deve registrar contoso.com do seu servidor. Quando você estiver fora do escritório, você pode abrir um navegador da web em seu notebook e o tipo **contoso.com** na caixa de texto de endereço para se conectar à instância do acesso via Web remoto que você configurou no Windows Server Essentials.  
  
###  <a name="BKMK_PersonalizedNames"></a>Entender os nomes de domínio Microsoft personalizada  
 Um nome de domínio personalizado do Microsoft inclui os seguintes recursos:  
  
-   Um nome de domínio personalizado para acesso via Web remoto (por exemplo, *yourhostname*. remotewebaccess.com). O nome de domínio é associado a seu endereço IP público.  
  
-   Uma dinâmica DNS atualizar o serviço de protocolo para que o acesso via Web remoto usando o nome de domínio não será interrompido se mudar seu endereço IP público. Normalmente, provedores de serviços de Internet (ISPs) para as conexões de banda larga s organização forneça dinâmicos endereços IP públicos que podem mudar.  
  
-   Um certificado confiável associado com o nome de domínio.  
  
 Para integrar um nome de domínio personalizado da Microsoft ao seu servidor, você precisa de uma conta da Microsoft (anteriormente conhecida como um Windows Live ID). Se você não tiver uma conta da Microsoft, você pode se inscrever para obter um no [Microsoft Hotmail](https://login.live.com/) site.  
  
> [!IMPORTANT]
>  Windows Live permite que caracteres especiais em sua senha de conta da Microsoft que o servidor não dá suporte. Se você usar um domínio personalizado da Microsoft, certifique-se de que sua senha de conta da Microsoft contém apenas os caracteres que o servidor der suporte. O servidor não dá suporte para uso de caracteres $, /, ' e %.  
  
###  <a name="BKMK_UseNewName"></a>Use um nome de domínio novos ou existentes  
 Para configurar automaticamente o nome de domínio em um servidor que executa o Windows Server Essentials, você deve usar um provedor de serviços de nome de domínio que esteja listado no Assistente de configuração de seu domínio nome. Você pode optar por receber um novo nome de domínio ou usar um nome de domínio existente. Siga um destes procedimentos:  
  
-   Se você quiser obter um novo nome de domínio de um domínio provedores de serviço de nomes que estão listados no assistente, clique em **eu quiser configurar um novo nome de domínio**.  
  
-   Se você tiver um nome de domínio existente que você comprou em um dos provedores de serviços de nome de domínio que têm suporte, você pode usar o Assistente de configuração de seu domínio nome para configurar o nome de domínio do seu servidor. Clique em **eu quiser usar um nome de domínio que já tenho**e, em seguida, digite o nome de domínio no **definir seu domínio nome** caixa de texto. Você deve fornecer o nome de usuário e senha que você usou para adquirir o nome de domínio.  
  
-   Se você tem um nome de domínio existentes que você comprou em um provedor de serviços de nome de domínio que não é compatível com o Windows Server Essentials e quiser usar o Assistente de configuração de seu domínio nome para configurar o nome de domínio do seu servidor, você pode transferir o nome de domínio para um dos provedores de serviços de nome de domínio listados no assistente. Clique em **eu quiser usar um nome de domínio que já tenho**, digite o nome de domínio no **nome de domínio** texto caixa e siga as instruções no site de s do provedor de serviço do domínio nome para transferir o nome de domínio.  
  
###  <a name="BKMK_SetUpName"></a>Configurar um nome de domínio  
 Quando você ativar o acesso via Web remoto, você pode optar por configurar o nome de domínio da Internet do servidor.  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>Configurar ou gerenciar um nome de domínio da Internet  
  
1.  Abra o painel.  
  
2.  Clique em **configurações do servidor**e, em seguida, clique no **acesso em qualquer local** guia.  
  
3.  No **nome de domínio** seção, clique em **configurar**.  
  
4.  Siga as instruções para concluir o assistente. Se você ainda não tem um nome de domínio e um certificado, o assistente ajuda você a encontrar um provedor de nomes de domínio para comprar um nome de domínio e um certificado, ou você pode obter um nome de domínio personalizado do Microsoft.  
  
###  <a name="BKMK_ChooseProvider"></a>Escolher um provedor de serviço de nomes de domínio  
 Você deve escolher um provedor de serviços de nome de domínio que dá suporte a extensão de nome de domínio que você deseja usar. Assistente de configuração de seu domínio nome inclui uma lista de provedores qualificados que você pode usar com um link para cada site do provedor s. Clique no **mais informações** link ao lado de cada nome de provedor s para obter informações sobre os serviços e os preços são oferecidos pelo fornecedor.  
  
> [!NOTE]
>  Alguns provedores de serviço de nomes de domínio têm amplos regiões internacionais e outros servem mercados menores. Por isso, alguns provedores podem não oferecer um site que é convertido em seu idioma de preferência.  
  
 Quando você compra o nome de domínio, você também pode considerar comprando o serviço de protocolo de atualização dinâmica do sistema de nome de domínio (DNS) de seu provedor de serviços de nome de domínio. Protocolo de atualização dinâmica DNS é um serviço que permite que qualquer pessoa na Internet obter acesso aos recursos em uma rede local quando o endereço IP de rede está constantemente mudando. Ou você pode adquirir um endereço IP estático do seu provedor de serviços de Internet (ISP) para garantir que seu endereço IP não muda.  
  
###  <a name="BKMK_ChooseDomainName"></a>Escolha um nome de domínio  
 Escolha um nome que identifica exclusivamente o seu servidor de negócios. Por exemplo, se seu nome de empresa for Contoso Ltd, você pode escolher Contoso para identificar exclusivamente o seu servidor doméstico ou empresarial na Internet. Se o nome de domínio não estiver disponível, tente outra variação de mesmo nome, ou talvez algo completamente diferente.  
  
 O nome digitado pode conter o seguinte:  
  
-   máximo de 63 caracteres  
  
-   Letras (inglês ou os caracteres localizados), números ou hifens (-). O nome deve começar e terminar com uma letra ou um número.  
  
    > [!NOTE]
    >  Nomes de domínio não diferenciam maiusculas de minúsculas.  
  
###  <a name="BKMK_Prefixes"></a>Escolha um prefixo de nome de domínio  
 Um nome de domínio consiste em rótulos hierárquicos.  
  
 **A extensão de domínio de nível superior** é o rótulo de extrema direita no nome do domínio. Por exemplo, no www.contoso.com, com é a extensão de nome de domínio de nível superior.  
  
 **O nome de domínio de segundo nível** é o rótulo ao lado da extensão de nome de domínio de nível superior. O nome de domínio de segundo nível geralmente é criado com base no nome da empresa, produtos ou serviços. Por exemplo, em www.contoso.com, contoso é o nome de domínio de segundo nível e foi escolhida para o nome da empresa Contoso produtos farmacêuticos. O domínio de segundo nível, às vezes, é conhecido como o nome do host, que tem um endereço IP associado a ele.  
  
 **O prefixo de nome de domínio** identifica um subdomínio. O nome de subdomínio pode ser usado para identificar os serviços, dispositivos ou regiões. Por exemplo, Contoso produtos farmacêuticos quer permitir que usuários remotos fazer logon no acesso via Web remoto, mas não quiser que o site esteja disponível para o público, portanto, eles criam um subdomínio que permite que somente usuários com as permissões adequadas para acessar o site. Contoso produtos farmacêuticos configura remote.contoso.com como o subdomínio e remoto é o prefixo de nome de domínio.  
  
> [!TIP]
>  É recomendável que você use o padrão **remoto** como o prefixo para seu nome de domínio.  
  
###  <a name="BKMK_Extension"></a>Escolha uma extensão de nome de domínio  
 Quando você escolhe um nome de domínio para o seu site da Internet, você também precisa especificar a extensão de nome de domínio que você deseja usar. A extensão é identificada pelas letras que seguem o período final de qualquer nome de domínio. (O termo formal para a extensão é o domínio de nível superior ou TLD.)  
  
 Existem dois tipos principais de extensões de domínio que você pode usar: genérico e o código do país.  
  
#### <a name="generic-top-level-domains"></a>Domínios de nível superior genéricos  
 Extensões de domínio genérico são três ou mais letras de comprimento, e eles são normalmente usados por determinados tipos de organizações.  
  
 **Exemplos de domínios de nível superior genéricos**  
  
|Extensão do domínio|Descrição|  
|----------------------|-----------------|  
|.com|Normalmente utilizado por organizações comerciais, mas ele pode ser usado por qualquer pessoa.|  
|.NET|Projetado para empresas que oferecem serviços de infraestrutura de rede.|  
|. org|Originalmente usado pelo agências sem fins lucrativos e outras empresas que não se enquadram em nenhuma outra categoria genérico de domínio de nível superior. Pode ser usado por qualquer pessoa.|  
|edu|Restrito para uso por organizações educacionais.|  
  
#### <a name="country-code-top-level-domains"></a>Domínios de nível superior do código do país  
 Essas extensões de domínio são duas letras de comprimento. Eles são projetados para ser usado por organizações no país ou região que está associado esse código. Alguns domínios de nível superior do código do país são restritos para uso por cidadãos desse país ou região. Outras estão disponíveis para uso por qualquer pessoa.  
  
 **Exemplos de domínios de nível superior do código do país**  
  
|Extensão do domínio|Descrição|  
|----------------------|-----------------|  
|. CA|Para uso por sites no Canadá|  
|. CN|Para uso por sites na China|  
|.de|Para uso por sites na Alemanha|  
|. co.uk|Para uso por sites no Reino Unido|  
  
 Para ver a lista completa de domínios de nível superior, consulte o [site Internet Assigned Numbers Authority](https://go.microsoft.com/fwlink/?LinkId=117438).  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>Se uma extensão de domínio não estiver disponível para selecionar em que o Assistente de configuração de nome de domínio  
 Quando você executa o Assistente de configuração de nome de domínio, o assistente analisa as informações do sistema para determinar seu país ou região. O assistente, em seguida, exibe apenas as extensões de domínio que dão suporte as provedores participantes em sua área. Se a extensão de domínio que você deseja não aparecer na lista, você deve escolher uma extensão de domínio diferente para continuar. Selecione uma extensão da lista que o assistente retornado.  
  
###  <a name="BKMK_UpdateService"></a>Atualizar ou atualize o seu serviço de nome de domínio  
 Você talvez precise atualizar ou atualizar seu serviço de nome de domínio, se você comprou um nome de domínio, mas não tiver comprado um certificado. Você deve ter um certificado para o nome de domínio do seu provedor de serviço de nome de domínio.  
  
> [!NOTE]
>  Trabalhar com seu provedor de serviços de nome de domínio para determinar o tipo de certificado que você precisa. O certificado pode ser um dos baratos certificados que são oferecidos. No entanto, você deve revisar a documentação e os recursos de certificados de segurança de nível superiores para determinar se eles atender melhor suas necessidades.  
  
###  <a name="BKMK_ExportCert"></a>Exportar ou importar o certificado no servidor  
 Se você quiser criar uma cópia de backup de um certificado ou usá-lo em outro servidor, você deve exportar o certificado. Para obter informações sobre como exportar certificados, consulte [exportar um certificado](https://go.microsoft.com/fwlink/p/?LinkId=214362).  
  
###  <a name="BKMK_SetNameManually"></a>Configurar um nome de domínio manualmente  
 Se você escolher essa opção, o servidor não monitorar ou manter o nome de domínio, e ele não alertá-lo se houver um problema de configuração. Você também pode considerar essa opção se qualquer um dos procedimentos a seguir for verdadeira:  
  
-   Não há provedores de nome de domínio do parceiro são listados para seu país ou região.  
  
-   Os provedores de domínio do parceiro listados não dão suporte a sua extensão de nome de domínio.  
  
-   Você tem um nome de domínio existente de um provedor de nomes de domínio que não é um parceiro no momento, e você não deseja transferir esse nome de domínio para um provedor de nomes de domínio que têm suporte do Windows Server Essentials.  
  
-   O assistente não lista a extensão de nome de domínio que você deseja usar, mas a extensão está disponível em um provedor de nomes de domínio que não é atualmente um parceiro.  
  
 Se você optar por configurar manualmente o nome de domínio, trabalhe com seu provedor de serviços de nome de domínio para criar um registro do seu domínio.  
  
##### <a name="to-create-an-a-record"></a>Para criar um registro  
  
1.  Decida sobre um nome de host, como o controle remoto. Este é o prefixo de nome de domínio. O prefixo de nome de domínio e seu nome de domínio definirão a URL para abrir a página de logon do acesso via Web remoto; Por exemplo, **http://remote.contoso.com**.  
  
2.  Em seu domínio nome serviço provedores configuração painel (geralmente na sua página da Web), crie o registro para o nome do host que você escolheu na etapa 1. Certifique-se de que o endereço IP que você especificar no registro é o endereço IP no lado WAN do roteador (Internet voltado para o lado). Consulte a documentação do roteador para localizar seu endereço IP WAN.  
  
3.  É recomendável que você entre em contato com seu provedor de serviços de Internet (ISP) para adquirir um endereço IP estático para sua rede. Isso garante que o endereço IP não será modificada e que a entrada DNS não ficar desatualizada.  
  
     Se você não tiver a opção para obter um endereço IP estático do seu provedor, você também pode considerar comprando o serviço de protocolo de atualização dinâmica do sistema de nome de domínio (DNS) de seu provedor de serviços de nome de domínio ou outro provedor de serviço. Protocolo de atualização dinâmica DNS é um serviço que mantém o endereço IP WAN sua rede atualizado para que o endereço IP pode ser resolvido para o nome de domínio, mesmo se o endereço IP muda.  
  
4.  Importe um certificado confiável quando o assistente avisa você. Se você não tiver um certificado confiável, você pode obtê-lo de um dos provedores de nome de domínio com suporte listados no assistente ou adquirir um do provedor de confiáveis de sua escolha. Para obter mais informações sobre um certificado confiável, contate seu provedor de nomes de domínio.  
  
###  <a name="BKMK_Find"></a>Encontre seu provedor de serviços de nome de domínio  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>Para encontrar o provedor de serviço de nomes de domínio para o nome de domínio  
  
1.  Abra um navegador da web e, em seguida, digite **www.internic.com** na barra de endereços para ir para a home page Internic.  
  
2.  Na home page de Internic, clique em **Whois**.  
  
3.  No **Whois** , digite o nome de domínio (por exemplo, contoso.com).  
  
4.  Clique no **domínio** opção e, em seguida, clique em **enviar**.  
  
5.  Nos resultados da pesquisa, o nome do seu provedor de serviços de nome de domínio está listado sob **registrador**.  
  
##  <a name="BKMK_4"></a>Personalizar o acesso via Web remoto  
 Você pode personalizar o seu site de acesso via Web remoto, adicionando uma imagem de logotipo ou em segundo plano pessoal. Você também pode adicionar links na página inicial para que essas informações estão disponíveis para todos os seus usuários. Para obter mais informações, consulte os seguintes tópicos:  
  
-   [Personalizar o acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [Personalizar imagens para telas de fundo e logotipos](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [Reparar o acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="BKMK_CustomizeRWA"></a>Personalizar o acesso via Web remoto  
 Você pode personalizar o acesso via Web remoto, alterando o título do site, alterando a imagem de plano de fundo e o logotipo e adicionar links a outros sites na home page.  
  
##### <a name="to-customize-remote-web-access"></a>Para personalizar o acesso via Web remoto  
  
1.  Abra o painel.  
  
2.  Clique em **configurações**e, em seguida, clique no **acesso em qualquer local** guia.  
  
3.  No **configurações do site** seção, clique em **personalizar**.  
  
4.  Quando você terminar de personalizar o acesso via Web remoto, clique em **Okey**. Teste suas alterações no acesso via Web remoto.  
  
###  <a name="BKMK_CustomizeImages"></a>Personalizar imagens para telas de fundo e logotipos  
 Esta seção fornece informações sobre as imagens que você pode usar para personalizar o acesso via Web remoto.  
  
#### <a name="image-size"></a>Tamanho da imagem  
 **Imagens de logotipo**  
  
 É recomendável que você use imagens de logotipo que têm 32 x 32 pixels. Imagens maiores são reduzidas para 32 x 32 e imagens menores são ampliadas para 32 x 32, que pode distorcer a imagem.  
  
 **Imagens de plano de fundo**  
  
 Embora não haja nenhum limite de tamanho para imagens de plano de fundo, para obter melhores resultados, é recomendável que você use imagens que são aproximadamente 800 x 500 pixels. A imagem de fundo é colocada no centro (horizontal e vertical) da página de logon. Para ajudar a tornar o texto na página de logon fácil de ler, o centro da imagem de plano de fundo deve ser ter cores claras.  
  
#### <a name="image-file-types"></a>Tipos de arquivo de imagem  
 Os seguintes tipos de arquivo de imagem podem ser usados para substituir o logotipo de site e tela de fundo padrão:  
  
-   Bitmap (bmp, \*.dib, \*.rle)  
  
-   GIF (GIF)  
  
-   PNG (*. png)  
  
-   JPG (jpg)  
  
###  <a name="BKMK_RepairRWA"></a>Reparar o acesso via Web remoto  
 O Assistente de reparo ajuda você a detectar e resolver problemas com seu roteador ou o nome de domínio. Há duas maneiras de descobrir problemas com acesso via Web remoto:  
  
-   Nas configurações do servidor no painel, na guia acesso em qualquer lugar, um ícone será exibido com um X vermelho, juntamente com uma descrição do problema.  
  
-   Um alerta no Visualizador de alerta.  
  
> [!NOTE]
>  O Assistente de reparo não está disponível até que você ativa o acesso via Web remoto. Para obter informações sobre como ativar o acesso via Web remoto, consulte [ativar o acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
##### <a name="to-repair-remote-web-access"></a>Para reparar o acesso via Web remoto  
  
1.  O painel de logon.  
  
2.  Clique em **configurações**e, em seguida, clique no **acesso em qualquer local** guia.  
  
3.  Clique em **reparo**. O **acesso via Web remoto de reparo** assistente é iniciado.  
  
4.  Clique em **próxima**. O assistente analisa o acesso via Web remoto, identifica o problema e, em seguida, tenta corrigir o problema.  
  
5.  Se você receber um alerta quando o assistente for concluído, você pode clicar em **repetir** para tentar corrigir o problema novamente. Se você continuar a receber um alerta, verifique o alerta para obter informações adicionais sobre o problema e as etapas de solução de problemas.  
  
##  <a name="BKMK_5"></a>Solucionar problemas de acesso via Web remoto  
  
-   [Solucionar problemas de conectividade de acesso via Web remoto](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [Solucionar problemas de seu firewall](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [Solucionar problemas de acesso em qualquer lugar](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>Consulte também  
  
-   [Opções de área de trabalho remotas](Remote-desktop-options.md)  
  
-   [Use o acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o acesso em qualquer lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
