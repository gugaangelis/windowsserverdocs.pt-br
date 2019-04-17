---
title: "Regras usadas pela ferramenta ferramenta Analisador de práticas recomendadas (BPA) do Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37e1dae7-586c-4dd7-bf83-7e14a9567c8f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c205bc8ff75bf64d4a13a7d799988c9d1ebe1a22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Regras usadas pela ferramenta ferramenta Analisador de práticas recomendadas (BPA) do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este artigo descreve as regras usadas pelo Windows Server Essentials BPA Best Practices Analyzer (). A ferramenta BPA examina um servidor que esteja executando o Windows Server Essentials e apresenta um relatório que descreve problemas e fornece recomendações para resolvê-los. As recomendações são desenvolvidas pela organização de suporte de produto do Windows Server Essentials.  
  
## <a name="using-the-tool"></a>Usando a ferramenta  
 É uma prática padrão, quando você migra para o Windows Server Essentials do Windows Server 2011 Essentials, Windows Small Business Server 2011 Essentials ou do Windows Home Server 2011, para executar a ferramenta BPA no servidor de destino depois de concluir a migração de suas configurações e dados. Você pode executar a ferramenta do painel a qualquer momento.  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>Para executar o Windows Server Essentials BPA no servidor  
  
1.  Fazer logon no servidor como administrador e, em seguida, abra o painel.  
  
2.  No painel, clique no **dispositivos** guia.  
  
3.  Sobre o **tarefas de servidor** painel, clique em **analisador de práticas recomendadas**.  
  
4.  Examine cada mensagem BPA e siga as instruções para resolver problemas se necessário.  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>Regras usadas pelo analisador de práticas recomendadas  
  
### <a name="disable-ip-filtering"></a>Desativar a filtragem de IP  
 **Problema:** filtragem IP atualmente está habilitado no servidor. Você deve desabilitar a filtragem IP.  
  
 **Impacto:** se filtragem IP está ativada, o tráfego de rede pode ser bloqueado.  
  
 **Resolução:**  
  
##### <a name="to-disable-ip-filtering"></a>Para desativar a filtragem de IP  
  
1.  Abra regedit.exe no servidor.  
  
2.  Navegue até HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters.  
  
3.  Clique com botão direito **EnableSecurityFilters**e clique em **modificar**.  
  
4.  No **Editar DWORD (32 bits) valor** janela, alterar o **dados do valor** campo como zero e, em seguida, clique em **Okey**.  
  
5.  Para aplicar a alteração, reinicie o servidor.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>O serviço Coordenador de transação distribuída (MSDTC) deve ser definido para ser iniciado automaticamente por padrão  
 **Problema:** o serviço MSDTC não está configurado para iniciar automaticamente  
  
 **Impacto:** o serviço MSDTC pode não ser iniciados automaticamente quando o servidor é iniciado. Se o serviço for interrompido, algumas funções do SQL Server ou COM poderá falhar. Como resultado, os aplicativos que usam o Microsoft SQL Server ou COM funções podem não funcionar corretamente.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>Para configurar o serviço MSDTC para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **coordenador de transações distribuídas** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automático (inicialização atrasada)**e clique em **Okey**.  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>O serviço de logon de rede deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** o serviço de logon de rede não está configurado para iniciar automaticamente.  
  
 **Impacto:** o serviço de logon de rede pode não iniciar automaticamente quando o servidor é iniciado. Se o serviço for interrompido, o servidor não pode autenticar usuários e serviços.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>Para configurar o serviço de logon de rede para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **Netlogon** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automática**e, em seguida, clique em **Okey**.  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>O serviço cliente DNS deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** o serviço de cliente DNS não está configurado para iniciar automaticamente.  
  
 **Impacto:** o serviço de cliente DNS não pode iniciar automaticamente quando o servidor é iniciado. Se esse serviço for interrompido, o servidor pode não ser capaz de resolver nomes DNS.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>Para configurar o serviço de cliente DNS para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **cliente DNS** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automática**e, em seguida, clique em **Okey**.  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>O serviço de servidor DNS deve ser configurado para serem iniciados automaticamente por padrão  
 **Problema:** o serviço de servidor DNS não está configurado para iniciar automaticamente.  
  
 **Impacto:** o serviço de servidor DNS não pode iniciar automaticamente quando o servidor é iniciado. Se esse serviço for interrompido, atualizações DNS não ocorrerá.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>Para configurar o serviço de servidor DNS para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **servidor DNS** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automática**e, em seguida, clique em **Okey**.  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Serviços do Active Directory da Web não está definido para o modo de tela inicial padrão  
 **Problema:** serviços do Active Directory da Web não está definido para o modo de tela inicial padrão do automático.  
  
 **Impacto:** serviços de Web do Active Directory (ADWS) não está definido para o modo de tela inicial padrão do automático. Se ADWS no servidor for interrompido ou desabilitado, aplicativos de cliente, como o módulo do Active Directory para Windows PowerShell ou o Centro Administrativo do Active Directory não é possível acessar ou gerenciar instâncias de serviço de diretório que estão em execução nesse servidor. Para obter mais informações, consulte [What's New no AD DS: serviços do Active Directory Web](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) na biblioteca técnica do Windows Server.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>Para configurar o serviço de serviços do Active Directory da Web para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **serviços do Active Directory Web** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automática**e, em seguida, clique em **Okey**.  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>O serviço cliente DHCP deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** o serviço cliente DHCP não está configurado para iniciar automaticamente.  
  
 **Impacto:** o serviço cliente DHCP não será iniciado automaticamente quando o servidor é iniciado. Se esse serviço for interrompido, os computadores cliente não podem receber um endereço IP do servidor.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>Para configurar o serviço cliente DHCP para ser iniciado automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **cliente DHCP** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automática**e, em seguida, clique em **Okey**.  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>O serviço de administração do IIS deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** o serviço de administração do IIS não está configurado para iniciar automaticamente.  
  
 **Impacto:** o serviço IIS Admin não será iniciado automaticamente quando o servidor é iniciado. Se esse serviço for interrompido, você talvez consiga acessar sites em execução no servidor, como acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>Para configurar o serviço de administração do IIS para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito **IIS Admin Service**e clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automática**e, em seguida, clique em **Okey**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>O serviço de publicação do World Wide Web deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** o serviço de publicação do World Wide Web não está configurado para iniciar automaticamente.  
  
 **Impacto:** o serviço de publicação do World Wide Web podem não ser iniciados automaticamente quando o servidor é iniciado. Se esse serviço for interrompido, você talvez consiga acessar sites em execução no servidor, como acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>Para configurar o serviço de publicação do World Wide Web para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito **serviço de publicação do World Wide Web**e clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automática**e, em seguida, clique em **Okey**.  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>O serviço Registro remoto deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** o serviço Registro remoto não está configurado para iniciar automaticamente.  
  
 **Impacto:**  
  
 O serviço Registro remoto não pode ser iniciados automaticamente quando o servidor é iniciado. Se esse serviço for interrompido, você talvez consiga executar algumas operações de rede remotamente.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>Para configurar o serviço Registro remoto para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **registro remoto** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automática**e, em seguida, clique em **Okey**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>O serviço de Gateway de área de trabalho remota deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** o serviço de Gateway de área de trabalho remota não está configurado para iniciar automaticamente.  
  
 **Impacto:** se esse serviço for interrompido, os usuários talvez não consigam acessar computadores usando o acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>Para configurar o serviço de Gateway de área de trabalho remota para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **Gateway de área de trabalho remota** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automático (inicialização atrasada)**e clique em **Okey**.  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>O serviço de tempo do Windows deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** o serviço de tempo do Windows não está configurado para iniciar automaticamente.  
  
 **Impacto:** se esse serviço for interrompido, sincronização de dados e a hora não estão disponíveis.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>Para configurar o serviço de tempo do Windows para iniciar automaticamente  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **tempo do Windows** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **geral**, altere o **tipo de inicialização** para **automática**e, em seguida, clique em **Okey**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>O serviço Distributed Transaction coordenador (MSDTC) deve ser iniciado  
 **Problema:** o serviço MSDTC não está em execução no servidor.  
  
 **Impacto:** se esse serviço é interrompido, algumas funções do SQL Server ou COM poderá falhar. Como resultado, os aplicativos que usam o Microsoft SQL Server ou COM funções podem não funcionar corretamente.  
  
 **Resolução:**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Para iniciar o serviço de coordenador de transações distribuídas  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **coordenador de transações distribuídas** serviço e, em seguida, clique em **iniciar**.  
  
### <a name="the-netlogon-service-should-be-started"></a>O serviço de logon de rede deve ser iniciado  
 **Problema:** o serviço de logon de rede não está em execução no servidor.  
  
 **Impacto:** se esse serviço não for iniciado, o servidor não pode autenticar usuários e serviços.  
  
 **Resolução:**  
  
##### <a name="to-start-the-netlogon-service"></a>Para iniciar o serviço de logon de rede  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **Netlogon** serviço e, em seguida, clique em **iniciar**.  
  
### <a name="the-dns-client-service-should-be-started"></a>O serviço de cliente DNS deve ser iniciado  
 **Problema:** o serviço de cliente DNS não está em execução no servidor.  
  
 **Impacto:** se esse serviço não for iniciado, o servidor pode ser incapaz de resolver nomes DNS.  
  
 **Resolução:**  
  
##### <a name="to-start-the-dns-client-service"></a>Para iniciar o serviço de cliente DNS  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **cliente DNS** serviço e, em seguida, clique em **iniciar**.  
  
### <a name="the-dns-server-service-should-be-started"></a>O serviço de servidor DNS deve ser iniciado  
 **Problema:** o serviço de servidor DNS não está em execução no servidor.  
  
 **Impacto:** se o serviço de servidor DNS não é iniciado, atualizações DNS não podem ocorrer.  
  
 **Resolução:**  
  
##### <a name="to-start-the-dns-server-service"></a>Para iniciar o serviço de servidor DNS  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **servidor DNS** serviço e, em seguida, clique em **iniciar**.  
  
### <a name="active-directory-web-services-is-not-started"></a>Serviços de Web do Active Directory não é iniciado  
 **Problema:** dos serviços Web do Active Directory não é iniciado.  
  
 **Impacto:** serviços de Web do Active Directory (ADWS) não é iniciado. Se ADWS no servidor for interrompido ou desabilitado, aplicativos de cliente, como o módulo do Active Directory para Windows PowerShell ou o Centro Administrativo do Active Directory não é possível acessar ou gerenciar instâncias de serviço de diretório que estão em execução nesse servidor. Para obter mais informações, consulte [What's New no AD DS: serviços do Active Directory Web](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) na biblioteca técnica do Windows Server.  
  
 **Resolução:**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>Para iniciar o serviço de serviços do Active Directory da Web  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito **serviços do Active Directory Web**e clique em **iniciar**.  
  
### <a name="the-dhcp-client-service-should-be-started"></a>O serviço cliente DHCP deve ser iniciado  
 **Problema:** o serviço cliente DHCP não está em execução no servidor.  
  
 **Impacto:** se esse serviço for interrompido, os computadores cliente não podem receber um endereço IP do servidor.  
  
 **Resolução:**  
  
##### <a name="to-start-the-dhcp-client-service"></a>Para iniciar o serviço cliente DHCP  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **cliente DHCP** serviço e, em seguida, clique em **iniciar**.  
  
### <a name="the-iis-admin-service-should-be-started"></a>O serviço de administração do IIS deve ser iniciado  
 **Problema:** o serviço de administração do IIS não está em execução no servidor.  
  
 **Impacto:** se esse serviço for interrompido, talvez consiga acessar sites em execução no servidor, como acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-start-the-iis-admin-service"></a>Para iniciar o serviço de administração do IIS  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito **IIS Admin Service**e clique em **iniciar**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>O serviço de publicação do World Wide Web deve ser iniciado  
 **Problema:** o serviço de publicação do World Wide Web não está em execução no servidor.  
  
 **Impacto:** se esse serviço for interrompido, talvez consiga acessar sites em execução no servidor, como acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>Para iniciar o serviço de publicação do World Wide Web  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito **serviço de publicação do World Wide Web**e clique em **iniciar**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>O serviço de Gateway de área de trabalho remota deve ser iniciado  
 **Problema:** o serviço de Gateway de área de trabalho remota não está em execução no servidor.  
  
 **Impacto:** se esse serviço for interrompido, os usuários talvez não consigam acessar computadores usando o acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>Para iniciar o serviço de Gateway de área de trabalho remota  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **Gateway de área de trabalho remota** serviço e, em seguida, clique em **iniciar**.  
  
### <a name="the-windows-time-service-should-be-started"></a>O serviço de tempo do Windows deve ser iniciado  
 **Problema:** o serviço de tempo do Windows não está em execução no servidor.  
  
 **Impacto:** se esse serviço é interrompido, sincronização de dados e a hora não será disponível.  
  
 **Resolução:**  
  
##### <a name="to-start-the-windows-time-service"></a>Para iniciar o serviço de tempo do Windows  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **tempo do Windows** serviço e, em seguida, clique em **iniciar**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>A conta de logon do serviço de coordenador de transação distribuída (MSDTC) deve ser NT \ serviço de rede  
 **Problema:** a conta de logon padrão para o serviço Coordenador de transação distribuída (MSDTC) for alterada.  
  
 **Impacto:** o serviço pode não ter as permissões que são necessários para funcionar como esperado. Como resultado, os aplicativos que usam o SQL Server ou COM funções podem não funcionar corretamente.  
  
 **Resolução:**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>Para alterar a conta de logon do serviço  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **coordenador de transações distribuídas** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **logon**, selecione **essa conta**, tipo **NT \ serviço**e, em seguida, clique em **Okey**.  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>O serviço de logon de rede deve usar a conta Sistema Local que sua conta de logon  
 **Problema:** a conta de logon padrão para o serviço de logon de rede é alterada.  
  
 **Impacto:** o serviço pode não ter as permissões que são necessários para funcionar como esperado. Como resultado, o servidor não pode autenticar usuários e serviços.  
  
 **Resolução:**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>Para alterar a conta de logon do serviço de logon de rede  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **Netlogon** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **logon**, selecione **conta Sistema Local**.  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>O serviço de cliente DNS deve usar a conta NT \ serviço de rede como sua conta de logon  
 **Problema:** a conta de logon padrão para o serviço de cliente DNS é alterada.  
  
 **Impacto:** o serviço pode não ter as permissões que são necessários para funcionar como esperado. Como resultado, o servidor pode ser incapaz de resolver nomes DNS.  
  
 **Resolução:**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>Para alterar a conta de logon do serviço de cliente DNS  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **cliente DNS** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **logon**, selecione **essa conta**e digite **NT \ serviço**.  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>O serviço de servidor DNS deve usar a conta Sistema Local que sua conta de logon  
 **Problema:** a conta de logon padrão para o serviço de servidor DNS é alterada.  
  
 **Impacto:** o serviço pode não ter as permissões que são necessários para funcionar como esperado. Como resultado, as atualizações DNS não podem ocorrer.  
  
 **Resolução:**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>Para alterar a conta de logon do serviço de servidor DNS  
  
1.  Abra services.msc no servidor  
  
2.  Clique com botão direito do **servidor DNS** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **logon**, selecione **conta Sistema Local**.  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Serviços de Web do Active Directory não é a conta de logon padrão  
 **Problema:** dos serviços Web do Active Directory não é a conta de logon padrão. Por padrão, a conta de logon é definida como **conta Sistema Local**.  
  
 **Impacto:** serviços de Web do Active Directory (ADWS) não é iniciado. Se ADWS no servidor for interrompido ou desabilitado, aplicativos de cliente, como o módulo do Active Directory para Windows PowerShell ou o Centro Administrativo do Active Directory não é possível acessar ou gerenciar instâncias de serviço de diretório que estão em execução nesse servidor. Para obter mais informações, consulte [What's New no AD DS: serviços do Active Directory Web](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) na biblioteca técnica do Windows Server.  
  
 **Resolução:**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>Para alterar a conta de logon de serviços do Active Directory da Web  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito **serviços do Active Directory Web**e clique em **propriedades**.  
  
3.  Alterar o **tipo de inicialização** para **automática**e clique em **Okey**.  
  
4.  Nos serviços de Web do Active Directory **propriedades**, clique no **logon** guia.  
  
5.  Selecione o **conta Sistema Local** opção e, em seguida, clique em **Okey**.  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>O serviço Windows Update deve usar a conta Sistema Local que sua conta de logon  
 **Problema:** a conta de logon padrão para o serviço atualizações automáticas for alterada.  
  
 **Impacto:** o serviço pode não ter as permissões que são necessários para funcionar como esperado. Como resultado, o servidor não pode receber atualizações automáticas.  
  
 **Resolução:**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>Para alterar a conta de logon do serviço Windows Update  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **Windows Update** serviço e, em seguida, clique em Propriedades.  
  
3.  Sobre o **logon**, selecione **conta Sistema Local**.  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>O serviço cliente DHCP deve usar a conta NT\LocalService como sua conta de logon  
 **Problema:** a conta de logon padrão para o serviço cliente DHCP for alterada.  
  
 **Impacto:** o serviço pode não ter as permissões que são necessários para funcionar como esperado. Como resultado, o computador cliente não receberá endereços IP do servidor.  
  
 **Resolução:**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>Para alterar a conta de logon do serviço cliente DHCP  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **cliente DHCP** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **logon**, selecione **essa conta**e, em seguida, digite **NT AUTHORITY\Local serviço**.  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>O serviço de administração do IIS deve usar a conta Sistema Local que sua conta de logon  
 **Problema:** a conta de logon padrão para o serviço de administração do IIS for alterada.  
  
 **Impacto:** o serviço pode não ter as permissões necessárias que são necessárias para funcionar como esperado. Como resultado, talvez você consiga acessar sites em execução no servidor, como acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-change-the-service-logon-account"></a>Para alterar a conta de logon do serviço  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito **serviço IIS Admin**e clique em **propriedades**.  
  
3.  Sobre o **logon**, selecione **conta Sistema Local**.  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>O serviço de publicação do World Wide Web deve usar a conta Sistema Local que sua conta de logon  
 **Problema:** a conta de logon padrão para o serviço de publicação do World Wide Web for alterada.  
  
 **Impacto:** o serviço pode não ter as permissões que são necessários para funcionar como esperado. Como resultado, talvez você consiga acessar sites em execução no servidor, como acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>Para alterar a conta de logon do serviço de publicação do World Wide Web  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito **serviço de publicação do World Wide Web**e clique em **propriedades**.  
  
3.  Sobre o **logon**, selecione **conta Sistema Local**.  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>O serviço de Gateway de área de trabalho remota deve usar a conta NT \ serviço de rede como sua conta de logon  
 **Problema:** a conta de logon padrão para o serviço de Gateway de área de trabalho remota for alterada.  
  
 **Impacto:** o serviço pode não ter as permissões adequadas para funcionar como esperado. Como resultado, os usuários não poderão acessar computadores usando o acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>Para alterar a conta de logon do serviço de Gateway de área de trabalho remota  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **Gateway de área de trabalho remota** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **logon**, selecione **essa conta**e digite **NT \ serviço**.  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>O serviço de tempo do Windows deve usar a conta NT \ serviço de rede como sua conta de logon  
 **Problema:** a conta de logon padrão para o serviço de tempo do Windows for alterada.  
  
 **Impacto:** o serviço pode não ter as permissões adequadas para funcionar como esperado. Como resultado, sincronização de data e hora pode não estar disponível.  
  
 **Resolução:**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>Para alterar a conta de logon do serviço de tempo do Windows  
  
1.  Abra services.msc no servidor.  
  
2.  Clique com botão direito do **tempo do Windows** serviço e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **logon**, selecione **essa conta**e, em seguida, digite **NT AUTHORITY\Local serviço**.  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>O grupo Administradores interno não tem o direito de fazer logon como um trabalho em lotes  
 **Problema:** grupo Administradores interno não tem o direito de fazer logon como um trabalho em lotes.  
  
 **Impacto:** se o administrador cria um alerta e configura o alerta a ser executada quando o administrador não estiver conectado, o alerta falhará com um código de erro de 2147943785.  
  
 **Resolução:** para obter informações sobre como dar os administradores internos permissão de grupo para fazer logon como um trabalho em lotes, consulte [conceder o direito de fazer logon como um trabalho em lotes de grupo administrador interno](https://technet.microsoft.com/library/jj635076) (https://technet.microsoft.com/library/jj635076).  
  
### <a name="the-windows-firewall-is-turned-off"></a>O Firewall do Windows está desativado  
 **Problema:** Firewall do Windows estiver desativado. O valor padrão é on.  
  
 **Impacto:** dependendo das suas configurações de firewall, o Firewall do Windows pode ajudar a proteger seu servidor e a rede de atividade maliciosa bloqueando algumas informações de passar por meio do servidor.  
  
 **Resolução:**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>Para ativar o Firewall do Windows no servidor  
  
1.  Abra o painel de controle no servidor.  
  
2.  No painel de controle, clique em **sistema e segurança**e clique em **Firewall do Windows**.  
  
3.  No Firewall do Windows, clique em **ativar ou desativar o Firewall do Windows**, selecione o **ativar o Firewall do Windows** opção e, em seguida, clique em **Okey**.  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>O adaptador de rede interno não está configurado para registrar o endereço IP no DNS  
 **Problema:** o adaptador de rede interno não está configurado para registrar seu endereço IP no DNS.  
  
 **Impacto:** se o endereço IP do adaptador de rede interno não estiver registrado no DNS, talvez não seja possível acessar o servidor usando o nome do computador servidor s.  
  
 **Resolução:** verificar se o adaptador de rede interno está configurado para registrar no DNS.  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS: Os valores da chave do registro DNS ForwardingTimeout e RecursionTimeout são idênticos  
 **Problema:** o valor da chave do registro DNS ForwardingTimeout não deve ser igual ao valor do do Registro RecursionTimeout.  
  
 **Impacto:** você não consiga acessar recursos de Internet pelo nome.  
  
 **Resolução:** definir o valor para a entrada do Registro RecursionTimeout seja maior que o valor da chave ForwardingTimeout, localizado no registro em HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters.  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>A zona DNS para frente de seu domínio do Active Directory não permitir atualizações seguras  
 **Problema:** você deve configurar a zona de pesquisa direta para permitir somente atualizações dinâmicas seguras.  
  
 **Impacto:** quando você habilitar atualizações dinâmicas seguras, somente usuários autorizados e hosts podem fazer alterações nos registros.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>Para configurar a zona de pesquisa direta para o domínio do Active Directory  
  
1.  Abra dnsmgmt.msc no servidor.  
  
2.  Clique com botão direito a zona de pesquisa direta para o domínio do Active Directory e clique em **propriedades**.  
  
3.  No **atualizações dinâmicas** lista suspensa, selecione **segura somente**e clique em **Okey**.  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>A zona DNS não permitir segura de encaminhamento atualizações  
 **Problema:** você deve configurar a zona de pesquisa direta para a zona _msdcs.* permitir que somente as atualizações dinâmicas.  
  
 **Impacto:** quando você habilitar atualizações dinâmicas seguras, somente usuários autorizados e hosts podem fazer alterações em registros na zona da msdcs.*.  
  
 **Resolução:**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>Para permitir atualizações seguras na zona _ msdcs  
  
1.  Abra dnsmgmt.msc no servidor.  
  
2.  Clique com botão direito a zona de pesquisa direta para a zona MSDCS e clique em **propriedades**.  
  
3.  No **atualizações dinâmicas** lista suspensa, selecione **segura somente**e clique em **Okey**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>A configuração de segurança aprimorada do Internet Explorer não está habilitada  
 **Problema:** Internet Explorer Enhanced Security Configuration (IE ESC) atualmente não está habilitado para o grupo Administradores.  
  
 **Impacto:** se a configuração de segurança aprimorada do Internet Explorer não está habilitada para o grupo Administradores, seu servidor e o Internet Explorer tem mais chances de ataques mal-intencionados que podem ocorrer por meio da Web conteúdo e scripts de aplicativos.  
  
 **Resolução:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Para habilitar a configuração de segurança aprimorada do Internet Explorer  
  
1.  Abrir **Gerenciador do servidor** no servidor e depois clique em **servidor Local**.  
  
2.  No **propriedades** painel, altere a configuração para **a configuração de segurança reforçada IE** para **em**e clique em **Okey**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>A configuração de segurança aprimorada do Internet Explorer não está habilitada  
 **Problema:** Internet Explorer Enhanced Security Configuration (IE ESC) atualmente não está habilitado para o grupo de usuários.  
  
 **Impacto:** se a configuração de segurança aprimorada do Internet Explorer não está habilitada para o grupo de usuários, seu servidor e o Internet Explorer tem mais chances de ataques mal-intencionados que podem ocorrer por meio da Web conteúdo e scripts de aplicativos.  
  
 **Resolução:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Para habilitar a configuração de segurança aprimorada do Internet Explorer  
  
1.  Abrir **Gerenciador do servidor**e clique em **servidor Local**.  
  
2.  No **propriedades** painel, altere a configuração para **a configuração de segurança reforçada IE** para **em**e clique em **Okey**.  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>O servidor de origem permanece em serviços e Sites do Active Directory  
 **Problema:** o servidor de origem que está executando o Windows Small Business Server ainda existe em serviços e Sites do Active Directory no Default-First-Site-Name.  
  
 **Impacto:** se o servidor de origem permanece em serviços e Sites do Active Diretor, computadores cliente podem experimentar conectividade problema: s.  
  
 **Resolução:** você deve rebaixar o servidor de origem, removê-lo do domínio e, em seguida, exclua o servidor de origem de Sites do Active Directory e serviços e Active Directory usuários e computadores.  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>Servidor de origem permanece no SBSComputer OU  
 **Problema:** o servidor de origem que está executando o Windows Small Business Server ainda existe no Active Directory usuários e computadores.  
  
 **Impacto:** se o servidor de origem permanece em Active Diretor de usuários e computadores, os computadores cliente podem experimentar conectividade problema: s.  
  
 **Resolução:** você deve rebaixar o servidor de origem, removê-lo do domínio e, em seguida, exclua o servidor de origem de Sites do Active Directory e serviços e Active Directory usuários e computadores.  
  
### <a name="a-group-policy-is-missing"></a>Uma política de grupo está ausente  
 **Problema:** a política de grupo da política de domínio padrão está ausente.  
  
 **Impacto:** a política de domínio padrão é necessária para funções de domínio correto.  
  
 **Resolução:**  
  
##### <a name="to-restore-a-missing-group-policy"></a>Para restaurar uma política de grupo ausente  
  
1.  Abra gpmc.msc no servidor.  
  
2.  No Gerenciador de política de grupo, expanda a floresta de domínio e pesquisar a árvore de console para o **política de domínio padrão** objeto de política de grupo.  
  
3.  Se a política não aparecer na árvore de, restaurá-lo a partir de um backup de estado do sistema.  
  
### <a name="no-dns-name-server-resource-records"></a>Nenhum DNS registros de recurso de servidor de nomes  
 **Problema:** não há nenhum registro de recurso de servidor (NS) de nome DNS na zona da pesquisa direta para o servidor.  
  
 **Impacto:** se nenhum registro de recurso de servidor (NS) de nome DNS existe na zona da pesquisa direta para o domínio do Active Directory, os usuários não poderão acessar recursos na rede ou na Internet.  
  
 **Resolução:**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>Para restaurar ausentes nome DNS server registros de recurso  
  
1.  Abra dnsmgmt.msc no servidor.  
  
2.  No Gerenciador DNS, clique com botão direito a zona de pesquisa direta para o domínio do Active Directory e clique em **propriedades**.  
  
3.  Sobre o **servidores de nomes** guia, verifique se as configurações estão corretas.  
  
4.  Faça as alterações necessárias e, em seguida, clique em **Okey** para salvar as configurações.  
  
### <a name="no-dns-name-server-records"></a>Nenhum registro de servidor de nome DNS  
 **Problema:** não há nenhum servidor de nome DNS (NS) registros de recurso na zona _ msdcs do seu servidor (por exemplo: _msdcs.contoso.local).  
  
 **Impacto:** não se existir nenhum registro de recurso de servidor (NS) de nome DNS na zona _ msdcs para o domínio do Active Directory, os usuários não poderão acessar recursos na rede ou na Internet.  
  
 **Resolução:**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>Para restaurar os registros de servidor de nome DNS ausentes  
  
1.  Abra dnsmgmt.msc no servidor.  
  
2.  No Gerenciador DNS, clique com botão direito a zona de pesquisa direta para a zona MSDCS e clique em **propriedades**.  
  
3.  Sobre o **servidores de nomes** guia, verifique se as configurações estão corretas.  
  
4.  Faça as alterações necessárias e, em seguida, clique em **Okey** para salvar as configurações.  
  
### <a name="no-dns-name-server-records"></a>Nenhum registro de servidor de nome DNS  
 **Problema:** não há nenhum servidor de nome DNS (NS) registros de recurso para o delegado msdcs encaminhar zona de pesquisa.  
  
 **Impacto:** se nenhum registro de recurso de servidor (NS) de nome DNS existe para o delegado msdcs encaminhar zona de pesquisa, o serviço de servidor DNS não possa resolver os registros de recurso para o domínio DNS e não será iniciado.  
  
 **Resolução:**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>Reconfigurar ausentes registros de servidor de nomes DNS  
  
1.  Abra dnsmgmt.msc no servidor.  
  
2.  No Gerenciador DNS, expanda o nome do servidor e, em seguida, **zonas de pesquisa direta**.  
  
3.  Clique na zona de pesquisa direta para o domínio do Active Directory (por exemplo: contoso. local).  
  
4.  A zona MSDCS delegadas aparece como uma pasta acinzentada. Clique com botão direito a zona MSDCS e clique em **propriedades**.  
  
5.  Sobre o **servidores de nomes** guia, verifique se as configurações estão corretas.  
  
6.  Faça as alterações necessárias e, em seguida, clique em **Okey** para salvar as configurações.  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>Usuários autenticados não é um membro do grupo acesso compatível do Windows 2000  
 **Problema:** o grupo usuários autenticados não é um membro do grupo acesso compatível do Windows 2000.  
  
 **Impacto:** se o grupo usuários autenticados interno não for um membro do grupo de acesso de compatível com versões anteriores ao Windows 2000, os usuários da rede poderá encontrar erros de "Acesso negado".  
  
 **Resolução:**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>Para adicionar usuários autenticados para o grupo de acesso de compatível com versões anteriores ao Windows 2000  
  
1.  Abra dsa.msc no servidor.  
  
2.  No **Builtin** pasta, clique com botão direito **acesso a compatíveis com o Windows 2000**e clique em **propriedades**.  
  
3.  Clique em **adicionar**, tipo **usuários autenticados**e clique em **Okey** duas vezes.  
  
### <a name="dns-client-not-configured"></a>Cliente DNS não configurada  
 **Problema:** o cliente DNS não está configurado para apontar apenas para o endereço IP interno do servidor.  
  
 **Impacto:** se o cliente DNS não estiver configurado para apontar apenas para o endereço IP interno do servidor, resolução de nomes DNS pode falhar.  
  
 **Resolução:**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>Para configurar o DNS para apontar apenas para o endereço IP interno de servidor s  
  
1.  No computador cliente, abra o **propriedades** página para a conexão de rede.  
  
2.  Certifique-se de que o DNS é configurado para apontar apenas para o endereço IP interno do servidor.  
  
### <a name="default-application-pool-value-changed"></a>Valor de Pool de aplicativos padrão foi alterado  
 **Problema:** o número máximo de processos de trabalho para o Pool de aplicativos DefaultAppPool não é definido como o valor padrão de 1.  
  
 **Impacto:** os usuários não poderão se conectar a serviços baseados na web do Windows Small Business Server.  
  
 **Resolução:**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>Para redefinir o máximo de processos de trabalho para o pool de aplicativos padrão  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e, em seguida, clique em **Pools de aplicativos**.  
  
3.  Em **Pools de aplicativos**, clique com botão direito **DefaultAppPool**e clique em **configurações avançadas**.  
  
4.  Em **configurações avançadas**, altere o valor para **processos máximo** como 1 e clique em **Okey**.  
  
5.  Fechar **configurações avançadas**, clique com botão direito **DefaultAppPool**e, em seguida, parar e reiniciar o pool de aplicativos.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>Pool de aplicativos para acesso via Web remoto não usa a conta padrão  
 **Problema:** o pool de aplicativos RemoteAppPool não está sendo executado com a conta padrão.  
  
 **Impacto:** os usuários da rede talvez não consiga acessar o site de acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>Para redefinir o pool de aplicativos remoto para usar a conta padrão  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e, em seguida, clique em **Pools de aplicativos**.  
  
3.  Em **Pools de aplicativos**, clique com botão direito **RemoteAppPool**e clique em **configurações avançadas**.  
  
4.  Em **configurações avançadas**, altere o **identidade** para **serviço de rede**e clique em **Okey**.  
  
5.  Fechar **configurações avançadas**, clique com botão direito **RemoteAppPool**e, em seguida, parar e reiniciar o pool de aplicativos.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>Pool de aplicativos para acesso via Web remoto não usa a versão do .NET Framework padrão  
 **Problema:** o pool de aplicativos RemoteAppPool não está sendo executado com a versão padrão do Microsoft .NET Framework.  
  
 **Impacto:** os usuários da rede talvez não consiga acessar o site de acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>Para alterar a versão do .NET Framework usada pelo RemoteAppPool  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e, em seguida, clique em **Pools de aplicativos**.  
  
3.  Em **Pools de aplicativos**, clique com botão direito **RemoteAppPool**e clique em **configurações avançadas**.  
  
4.  Em **configurações avançadas**, altere o **.NET Framework versão** v 4.0, e depois clique em **Okey**.  
  
5.  Fechar **configurações avançadas**, clique com botão direito **RemoteAppPool**e, em seguida, parar e reiniciar o pool de aplicativos.  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>O arquivo RemoteAccess.log é maior que 1 GB de tamanho  
 **Problema:** se o tamanho do arquivo Remoteaccess.log exceder 1 GB, você pode enfrentar erros de espaço em disco insuficiente na unidade do sistema.  
  
 **Impacto:** se o arquivo Remoteaccess.log for muito grande, ele pode fazer com que o espaço livre problema: s na unidade c:.  
  
 **Resolução:** depois de fazer backup do servidor, você pode excluir o arquivo Remoteaccess.log, que está localizado na pasta Server\Logs\WebApps %ProgramData%\Microsoft\Windows.  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>Diretório de log do site padrão é mais de 1 GB  
 **Problema:** se o tamanho da pasta de log do site padrão exceder 1 GB, você pode enfrentar erros de espaço em disco insuficiente na unidade do sistema.  
  
 **Impacto:** se a pasta de log do site padrão for muito grande, ele pode fazer com que o espaço livre problema: s na unidade c:  
  
 **Resolução:** depois que você faça backup do servidor e enquanto o site padrão é interrompido, você pode excluir os arquivos de log na pasta C:\inetpub\logs\LogFiles\W3SVC1. Em seguida, inicie o site padrão.  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>Nenhuma associação para SSL em todos os endereços IP  
 **Problema:** não há nenhuma associação para Secure Sockets Layer (SSL) em todos os endereços IP no servidor.  
  
 **Impacto:** se SSL não está vinculado a todos os endereços IP no servidor, alguns sites não estarão disponíveis para os usuários.  
  
 **Resolução:**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>Para associar SSL para todos os endereços IP no servidor  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, sobre o **conexões** painel, expanda seu servidor, **Sites**, clique com botão direito **site padrão**e clique em **Editar vinculações**.  
  
3.  Em **associações de Site**, clique em **adicionar**e, em seguida, selecione as seguintes configurações:  
  
    -   **Tipo** = **https**  
  
    -   **Endereço IP** = **todos não atribuídos**  
  
    -   **Porta** = 443  
  
4.  Selecione um certificado SSL e clique em **Okey** para salvar as alterações.  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>Nenhuma associação para SSL no site padrão  
 **Problema:** não há nenhuma associação para SSL no site padrão.  
  
 **Impacto:** se o SSL não está vinculado ao site padrão, alguns sites podem não estar disponíveis para os usuários.  
  
 **Resolução:**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>Para associar SSL para o site padrão  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, sobre o **conexões** painel, expanda seu servidor, **Sites**, clique com botão direito **site padrão**e clique em **Editar vinculações**.  
  
3.  Em **associações de Site**, clique em **adicionar**e, em seguida, selecione as seguintes opções:  
  
    -   **Tipo** = **https**  
  
    -   **Endereço IP** = **todos não atribuídos**  
  
    -   **Porta** = 443  
  
    > [!NOTE]
    >  Se existe uma ligação de HTTPS para a porta 443 para um endereço IP específico, alterar o **endereço IP** atributo para essa ligação para **todos os não atribuídos**. A exceção a isso é para o endereço IP 127.0.0.1. Não altere a associação para 127.0.0.1.  
  
4.  Selecione um certificado SSL e clique em **Okey** para salvar as alterações.  
  
### <a name="a-certificate-expires-within-30-days"></a>Um certificado expirar dentro de 30 dias  
 **Problema:** seu certificado do servidor expirará dentro de 30 dias.  
  
 **Impacto:** o servidor não pode usar um certificado expirado. Se o certificado expirar, os usuários não poderão usar funções de acesso em qualquer local.  
  
 **Resolução:** para impedir que o certificado expirar, renovar o certificado com sua autoridade de certificação confiável.  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>Entidade do certificado não coincide com o nome configurado pelo Assistente de nome de domínio  
 **Problema:** entidade do certificado não corresponder ao nome que foi configurado pelo Assistente de nome de domínio.  
  
 **Impacto:** se a entidade do certificado não corresponder ao nome que foi configurado pelo Assistente de nome de domínio, alguns sites não inicializará. Outros sites exibirá o erro "Há um problema com o certificado de segurança deste site."  
  
 **Resolução:** para resolver esse problema:, execute o Assistente de acesso em qualquer lugar de configurar novamente e forneça o nome de domínio correto para o certificado ou comprar um novo certificado que corresponda ao nome de domínio que você deseja usar.  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>Uma ou mais contas de usuário têm nomes CN duplicados  
 **Problema:** uma ou mais contas de usuário têm nomes duplicados do CN: {0}.  
  
 **Impacto:** se contas de usuário têm nomes duplicados de CN, os usuários não poderão fazer logon na rede. Além disso, pesquisas do Active Directory para os usuários podem retornar valores incorretos.  
  
 **Resolução:** para resolver esse problema:, certifique-se de que as contas de usuário de rede não têm duplicar "CN =" nomes. Para tornar mais fácil, considere a possibilidade de exportar o conteúdo do Active Directory para um arquivo de texto para revisão. Para obter informações sobre como fazer isso, consulte [usando LDIFDE para importar e exportar objetos de diretório para o Active Directory (artigo da Base de Conhecimento 237677)](https://support.microsoft.com/kb/237677) (https://support.microsoft.com/kb/237677).  
  
### <a name="nt-backup-is-installed"></a>Backup do NT está instalado  
 **Problema:** o programa Backup do Windows NT é instalado no servidor.  
  
 **Impacto:** Windows Server Essentials usa o Backup do Windows Server. Se o programa Backup do Windows NT também é instalado, podem existir conflitos entre os dois programas de backup. Isso pode causar o falha do processo de Backup do Windows Server. Os conflitos também podem impedi-lo de usar um backup para restaurar o servidor.  
  
 **Resolução:** para resolver esse problema:, desinstale o programa Backup NT do servidor.  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>IIS não possui porta 80 (0.0.0.0:80) ou porta 443 (0.0.0.0:443)  
 **Problema:** Internet Information Services (IIS) não possui porta 80 (0.0.0.0:80) ou 443. Estas portas atualmente são vinculadas por outros aplicativos.  
  
 **Impacto:** aplicativos do Windows Server Essentials web exigir o uso de portas 80 e porta 443 para disponibilizar os serviços para os usuários. Se outro processo ou aplicativo já estiver usando a porta 80 ou 443, não é possível executar os aplicativos de web do Windows Server Essentials. Se isso ocorrer, acesso via Web remoto e outros aplicativos não estão disponíveis para os usuários.  
  
 **Resolução:** para resolver esse problema:, nenhuma desinstalar o aplicativo que já esteja usando porta 80 ou 443, ou atribuir o aplicativo a uma porta diferente.  
  
### <a name="the-default-website-is-not-running"></a>O site padrão não está em execução  
 **Problema:** do site padrão não estiver em execução em seu ambiente Windows Server Essentials.  
  
 **Impacto:** aplicativos do Windows Server Essentials web exigem o uso do site padrão. Se o site padrão não está em execução, acesso via Web remoto e outros aplicativos não estão disponíveis para os usuários.  
  
 **Resolução:**  
  
##### <a name="to-start-the-default-website"></a>Na tela inicial do site padrão  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e, em seguida, clique em **Sites**.  
  
3.  Clique com botão direito **site padrão**, aponte para **gerenciar site**e clique em **iniciar**.  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>Permissões de leitura e o Script para o diretório virtual /Remote estiverem incorretas  
 **Problema:** permissões de leitura e Script não são atribuídas ao diretório /Remote virtual.  
  
 **Impacto:** se as permissões de leitura e o Script para o diretório virtual /Remote estiverem incorretas, os usuários não podem usar acesso via Web remoto. Quando eles tentam usar acesso via Web remoto para navegar na Internet, ele podem encontrar o erro "Erro HTTP 403.1 proibido".  
  
 **Resolução:**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>Para atribuir permissões de leitura e o Script para o diretório /Remote  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e, em seguida, clique em **Sites**.  
  
3.  Expanda **site padrão**e, em seguida, expanda **remoto**.  
  
4.  Em **modo de exibição de recursos**, clique duas vezes em **mapeamentos de manipulador**.  
  
5.  Sobre o **ações** painel, clique em **editar permissões de recurso**.  
  
6.  Selecione o **leitura** e **Script** caixas de seleção e, em seguida, clique em **Okey**.  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>Redirecionamento HTTP está definido ou herdado no diretório virtual /Remote  
 **Problema:** o atributo de redirecionamento HTTP é definido ou herdado no diretório virtual /Remote inesperadamente.  
  
 **Impacto:** se o atributo de redirecionamento HTTP é definido no diretório virtual /Remote, local de trabalho remota da Web não funcionará corretamente.  
  
 **Resolução:**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>Para remover o atributo de redirecionamento HTTP  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e, em seguida, clique em **Sites**.  
  
3.  Expanda **site padrão**e, em seguida, expanda **remoto**.  
  
4.  Em **modo de exibição de recursos**, clique duas vezes em **redirecionamento HTTP**.  
  
5.  Limpar o **redirecionar solicitações para esse destino** caixa de seleção e, em seguida, clique em **aplicar** sobre o **ações** painel.  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>Existe um nome de host para porta 80 no site da Web padrão  
 **Problema:** um nome de host é atribuído para a porta 80 no site do padrão.  
  
 **Impacto:** se um nome de host é atribuído para a porta 80 no site do padrão, você não consiga se conectar a alguns aplicativos de web do Windows Server Essentials. Um nome de host não é obrigatório e não é recomendado nesta situação  
  
 **Resolução:**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>Para limpar a entrada de nome de host de porta 80 no site da Web padrão  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e, em seguida, clique em **Sites**.  
  
3.  Em **modo de exibição de recursos**, clique com botão direito **site padrão**e clique em **associações**.  
  
4.  Em **associações de Site**, selecione o **http para porta 80** definindo e, em seguida, clique em **editar**.  
  
5.  Em **Editar Site Binding**, desmarque o **nome do Host** entrada e clique em **Okey**.  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>Backup não acontece por causa de uma partição oculta  
 **Problema:** uma partição NTFS não está agendada para backup pelo Backup do Windows Server.  
  
 **Impacto:** Backup do Windows Server só pode fazer backup partições formatadas como NTFS.  
  
 **Resolução:** não configure o Backup do Windows Server para fazer backup de partições não NTFS. Para obter mais informações, consulte [12290 de IDs de evento e 16387 são registrados quando backup do estado do sistema Falha em um computador baseado no Windows Server 2008 (artigo da Base de Conhecimento 968128)](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128).  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>O backup mais recente não teve êxito.  
 **Problema:** a tentativa de backup mais recente não foi concluída com êxito.  
  
 **Impacto:** o status de backup para o sistema não está correto.  
  
 **Resolução:** examine os logs de eventos e logs de backup para erros que ocorreram durante o backup mais recente.  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>O tipo de inicialização para o serviço de replicação de arquivo não é definido como automático  
 **Problema:** o serviço de duplicação de arquivos (FRS) talvez não seja iniciado se o tipo de inicialização não é definido como o valor padrão de automático.  
  
 **Impacto:** se não estiver executando o serviço de duplicação de arquivos, o controlador de domínio pode parar de seus serviços de publicidade. Isso pode levar a outros problemas, como erros de logon e erros de política de grupo.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>Para configurar o serviço de duplicação de arquivos para a inicialização automática  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **File Replication**.  
  
3.  Para **tipo de inicialização**, selecione **automática**e clique em **aplicar**.  
  
### <a name="the-file-replication-service-is-not-running"></a>O serviço de replicação de arquivo não está funcionando  
 **Problema:** o serviço de replicação de arquivo não está funcionando.  
  
 **Impacto:** se não estiver executando o serviço de duplicação de arquivos, o controlador de domínio pode parar de seus serviços de publicidade. Esse comportamento pode causar outros problemas, como erros de logon e erros de política de grupo.  
  
 **Resolução:**  
  
##### <a name="to-start-the-file-replication-service"></a>Para iniciar o serviço de replicação de arquivo  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **serviço de replicação de arquivo**.  
  
3.  Clique em **iniciar**.  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>A conta de logon para o serviço de replicação de arquivo não é configurada para usar a conta Sistema Local  
 **Problema:** o serviço de duplicação de arquivos não está configurado para usar a conta Sistema Local como a conta de logon padrão.  
  
 **Impacto:** se o serviço de duplicação de arquivos não usa o sistema Local como a conta de logon padrão, você pode encontrar erros relacionados a permissões. Esses erros podem acionar outros erros e, eventualmente, podem fazer com que o controlador de domínio parar de seus serviços de publicidade.  
  
 **Resolução:**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>Para configurar o sistema Local como a conta de logon padrão para replicação de arquivo  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **File Replication**.  
  
3.  Sobre o **propriedades do serviço** página, clique no **logon** guia.  
  
4.  Selecione o **conta Sistema Local** opção e, em seguida, clique em **aplicar**.  
  
5.  Reinicie o serviço.  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>O tipo de inicialização para o serviço de replicação DFS não é definido como automático  
 **Problema:** o serviço de replicação DFS talvez não seja iniciado se o tipo de inicialização não é definido como o valor padrão de automático.  
  
 **Impacto:** se o serviço de replicação DFS não está em execução, o controlador de domínio pode parar de seus serviços de publicidade. Isso pode levar a outros problemas, como erros de logon e erros de política de grupo.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>Para configurar o serviço de replicação DFS para inicialização automática  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **replicação DFS**.  
  
3.  Para **tipo de inicialização**, selecione **automática**e clique em **aplicar**.  
  
### <a name="the-dfs-replication-service-is-not-running"></a>O serviço de replicação DFS não está funcionando  
 **Problema:** o serviço de replicação DFS não estiver sendo executado.  
  
 **Impacto:** se o serviço de replicação DFS não está em execução, o controlador de domínio pode parar de seus serviços de publicidade. Esse comportamento pode causar outros problemas, como erros de logon e erros de política de grupo.  
  
 **Resolução:**  
  
##### <a name="to-start-the-dfs-replication-service"></a>Para iniciar o serviço de replicação DFS  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **replicação DFS**.  
  
3.  Clique em **iniciar**.  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>O serviço não é a replicação DFS não está definida para usar a conta Sistema Local  
 **Problema:** o serviço de replicação DFS não está definido para usar a conta Sistema Local como a conta de logon padrão.  
  
 **Impacto:** se o serviço de replicação DFS não usa o sistema Local como a conta de logon padrão, você pode encontrar erros relacionados a permissões. Esses erros podem acionar outros erros e, eventualmente, podem fazer com que o controlador de domínio parar de seus serviços de publicidade.  
  
 **Resolução:**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>Para configurar a replicação DFS para usar o sistema Local como a conta de logon padrão  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **replicação DFS**.  
  
3.  Sobre o **propriedades do serviço** página, clique no **logon** guia.  
  
4.  Selecione o **conta Sistema Local** opção e, em seguida, clique em **aplicar**.  
  
5.  Reinicie o serviço.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>O serviço de integração do Windows Server Office 365 não está configurado para usar a conta Sistema Local  
 **Problema:** o serviço de integração do Windows Server Office 365 não está definido para usar a conta Sistema Local como a conta de logon padrão.  
  
 **Impacto:** se o serviço de integração do Windows Server Office 365 não usa o sistema Local como a conta de logon padrão, alguns recursos do Office 365 podem não funcionar corretamente. Você também poderá encontrar erros relacionados a permissões.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>Para configurar o serviço de integração do Office 365 para usar o sistema Local como a conta de logon padrão  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **o serviço de integração do Windows Server Office 365**.  
  
3.  Sobre o **propriedades do serviço** página, clique no **logon** guia.  
  
4.  Selecione o **conta Sistema Local** opção e, em seguida, clique em **aplicar**.  
  
5.  Reinicie o serviço.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>O serviço de integração do Windows Server Office 365 não está em execução  
 **Problema:** o serviço de integração do Windows Server Office 365 não está em execução no momento.  
  
 **Impacto:** se não estiver executando o serviço de integração do Windows Server Office 365, os recursos do Office 365 baseado em nuvem não estão disponíveis.  
  
 **Resolução:**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>Para iniciar o serviço de integração do Windows Server Office 365  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **o serviço de integração do Windows Server Office 365**.  
  
3.  Clique em **iniciar**.  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>O tipo de inicialização para o serviço de integração do Windows Server Office 365 não é definido como automático  
 **Problema:** o serviço de integração do Windows Server Office 365 talvez não seja iniciado se o tipo de inicialização não é definido como o valor padrão de automático.  
  
 **Impacto:** se não estiver executando o serviço de integração do Windows Server Office 365, os recursos do Office 365 baseado em nuvem não estão disponíveis.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>Para configurar o serviço de integração do Office 365 para inicialização automática  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **o serviço de integração do Windows Server Office 365**.  
  
3.  Para **tipo de inicialização**, selecione **automática**e clique em **aplicar**.  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>Um valor do registro está ausente ou definido incorretamente  
 **Problema:** uma chave do registro em HKEY_LOCAL_MACHINE \Software\Microsoft\Rpc\RpcProxy contém valores incorretos ou não existe.  
  
 **Impacto:** caso a chave de registro RPCProxy está definida incorretamente, você poderá receber uma mensagem de erro que se parece com o seguinte: "o computador não pode se conectar ao computador remoto porque o servidor de Gateway de área de trabalho remota não está disponível temporariamente. Tente reconectar mais tarde ou entre em contato com o administrador de rede para obter assistência."  
  
 **Resolução:**  
  
##### <a name="to-correct-the-registry-setting"></a>Para corrigir a configuração do registro  
  
1.  Abra o Editor do registro.  
  
2.  Navegue até a seguinte chave do registro:  
  
     HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy  
  
3.  Certifique-se de que a cadeia de caracteres chamada "Site" tem um valor de dados de site padrão:  
  
    -   Se o valor de dados estiver incorreto, modifique a cadeia de caracteres para usar o valor correto.  
  
    -   Se a cadeia de caracteres não existir, crie uma nova cadeia de caracteres chamada "Site" e defina o valor de dados para o site padrão."  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>O tipo de inicialização para o serviço de mecanismo de Backup de nível de bloco não é definido como Manual  
 **Problema:** o serviço de mecanismo de Backup de nível de bloco não é usando o tipo de inicialização padrão do Manual.  
  
 **Impacto:** o serviço de mecanismo de Backup de nível de bloco talvez não seja iniciado se o tipo de inicialização não é definido como Manual. Esse problema: podem causar trabalhos de Backup do Windows Server falhar.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>Para configurar o serviço do mecanismo de Backup do nível de bloco para inicialização manual  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **serviço do mecanismo de Backup de nível de bloco**.  
  
3.  Para **tipo de inicialização**, selecione **Manual**e clique em **aplicar**.  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>A conta de logon para o serviço de mecanismo de Backup de nível de bloco não é configurada para usar a conta Sistema Local  
 **Problema:** o serviço de mecanismo de Backup de nível de bloco não estiver definido para usar a conta Sistema Local como a conta de logon padrão.  
  
 **Impacto:** se o serviço do mecanismo de Backup de nível de bloco não usa o sistema Local como a conta de logon padrão, você pode encontrar erros relacionados a permissões. Esses erros podem evitar que os trabalhos de Backup do Windows Server concluída com êxito.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>Para configurar o serviço para usar o sistema Local como a conta de logon padrão do mecanismo de Backup de nível de bloco  
  
1.  Abra o console de serviços.  
  
2.  Na lista de serviços, clique duas vezes em **serviço do mecanismo de Backup de nível de bloco**.  
  
3.  Sobre o **propriedades do serviço** página, clique no **logon** guia.  
  
4.  Selecione o **conta Sistema Local** opção e, em seguida, clique em **aplicar**.  
  
5.  Reinicie o serviço.  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>O nome comum do certificado que é vinculado ao site do serviço de Web do WSS certificado não coincide com o nome do servidor  
 **Problema:** um certificado não válido é associado ao site do serviço de Web do WSS certificado no IIS. O nome comum nesse certificado não coincide com o nome do servidor.  
  
 **Impacto:** se você associar um certificado não é válido para o site do serviço de Web do WSS certificado, o assistente conectar talvez não funcionem corretamente.  
  
 **Resolução:**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>Para configurar um certificado válido para o serviço de Web do WSS certificado  
  
1.  Abra o Gerenciador de serviços de informações da Internet (IIS) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e, em seguida, clique em **Sites**.  
  
3.  Clique com botão direito **serviço de Web do WSS certificado**e clique em **Editar vinculações**.  
  
4.  Em **associações de Site**, clique em **HTTPS**e clique em **editar**.  
  
5.  Em **Editar Site Binding**, para **certificado SSL**, selecione o certificado que tem o mesmo nome de seu servidor.  
  
6.  Se mais de uma entrada de certificado tem o mesmo nome de seu servidor, clique em **exibição** para determinar qual certificado é válido e, em seguida, selecione o certificado apropriado.  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>Parece ser um problema com a vinculação de certificado para o serviço de Gateway de área de trabalho remota  
 **Problema:** o certificado para o serviço de Gateway de área de trabalho remota parece ser vinculados incorretamente.  
  
 **Impacto:** se o certificado para o serviço de Gateway de área de trabalho remota não está configurado corretamente, os usuários não podem se conectar para acesso via Web remoto.  
  
 **Resolução:**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>Para corrigir a associação para o serviço de Gateway de área de trabalho remota  
  
-   Abra um prompt de comando como administrador e digite os seguintes comandos:  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     Para obter mais informações, consulte [como gerenciar o serviço de Gateway de área de trabalho remota no Windows Server Essentials (artigo da Base de Conhecimento 2472211)](https://support.microsoft.com/kb/2472211) (https://support.microsoft.com/kb/2472211).
