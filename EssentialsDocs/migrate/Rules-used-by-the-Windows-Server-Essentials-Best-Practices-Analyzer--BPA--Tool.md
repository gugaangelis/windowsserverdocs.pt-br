---
title: As regras usadas pela Ferramenta Analisador de Práticas Recomendadas (BPA) do Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850687"
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>As regras usadas pela Ferramenta Analisador de Práticas Recomendadas (BPA) do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este artigo descreve as regras usadas pelo Windows Server Essentials BPA Best Practices Analyzer (). O BPA examina o servidor que está executando o Windows Server Essentials e apresenta um relatório que descreve os problemas e fornece recomendações para resolvê-los. As recomendações são desenvolvidas pela organização de suporte do produto para o Windows Server Essentials.  
  
## <a name="using-the-tool"></a>Usando a ferramenta  
 É uma prática padrão, quando você migra para o Windows Server Essentials do Windows Server 2011 Essentials, Windows Small Business Server 2011 Essentials ou Windows Home Server 2011, executar o BPA no servidor de destino depois de concluir a migração de sua as configurações e dados. Você pode executar a ferramenta pelo Painel a qualquer momento.  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>Para executar o BPA do Windows Server Essentials no servidor  
  
1.  Faça logon no servidor como administrador e abra o Painel.  
  
2.  No painel, clique na guia **Dispositivos**.  
  
3.  No painel **Tarefas do Servidor**, clique em **Analisador de Práticas Recomendadas**.  
  
4.  Examine cada mensagem BPA e siga as instruções para resolver problemas, se necessário.  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>Regras usadas pelo Analisador de Práticas Recomendadas  
  
### <a name="disable-ip-filtering"></a>Desabilitar a filtragem de IP  
 **Problema:** A filtragem de IP está habilitada no servidor. Você deve desabilitar a filtragem de IP.  
  
 **Impacto:** Se a filtragem de IP estiver habilitada, o tráfego de rede poderá estar bloqueado.  
  
 **Resolução:**  
  
##### <a name="to-disable-ip-filtering"></a>Para desabilitar a filtragem de IP  
  
1.  Abra o regedit.exe no servidor.  
  
2.  Navegue até HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters.  
  
3.  Clique com o botão direito do mouse em **EnableSecurityFilters** e clique em **Modificar**.  
  
4.  Na janela **Editar Valor DWORD (32 bits)**, altere o campo **Dados do valor** para zero e clique em **OK**.  
  
5.  Para aplicar a alteração, reinicie o servidor.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>O serviço MSDTC (Coordenador de Transações Distribuídas) deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** O serviço MSDTC não está configurado para iniciar automaticamente  
  
 **Impacto:** O serviço MSDTC talvez não seja iniciado automaticamente quando o servidor for iniciado. Se o serviço for interrompido, algumas funções do SQL Server ou COM podem falhar. Como resultado, os aplicativos que usam funções do Microsoft SQL Server ou COM talvez não funcionem corretamente.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>Para configurar o serviço MSDTC para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com o botão direito do mouse no serviço **Coordenador de Transações Distribuídas** e em **Propriedades**.  
  
3.  Na guia **Geral**, altere o **Tipo de inicialização** para **Automático (Atraso na Inicialização)** e clique em **OK**.  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>O serviço Netlogon deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** O serviço Netlogon não está configurado para iniciar automaticamente.  
  
 **Impacto:**  O serviço Netlogon talvez não seja iniciado automaticamente quando o servidor for iniciado. Se o serviço for interrompido, talvez o servidor não autentique usuários nem serviços.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>Para configurar o serviço Netlogon para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Netlogon** e clique em **Propriedades**.  
  
3.  Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>O serviço Cliente DNS deve ser configurado para iniciar automaticamente por padrão  
 **Problema:**  O serviço Cliente DNS não está configurado para iniciar automaticamente.  
  
 **Impacto:**  O serviço Cliente DNS talvez não seja iniciado automaticamente quando o servidor for iniciado. Se esse serviço for interrompido, talvez o servidor não consiga resolver nomes DNS.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>Para configurar o serviço Cliente DNS para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Cliente DNS** e clique em **Propriedades**.  
  
3.  Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>O serviço Servidor DNS deve ser configurado para iniciar automaticamente por padrão  
 **Problema:**  O serviço Servidor DNS não está configurado para iniciar automaticamente.  
  
 **Impacto:**  O serviço Servidor DNS talvez não seja iniciado automaticamente quando o servidor for iniciado. Se esse serviço for interrompido, as atualizações de DNS não serão realizadas.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>Para configurar o serviço Servidor DNS para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Servidor DNS** e clique em **Propriedades**.  
  
3.  Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Os Serviços Web do Active Directory não estão configurados para o modo de inicialização padrão  
 **Problema:**  Os Serviços Web do Active Directory não estão configurados para o modo de inicialização padrão de Automático.  
  
 **Impacto:**  Os ADWS (Serviços Web do Active Directory) não estão configurados para o modo de inicialização padrão de Automático. Se os ADWS no servidor forem interrompidos ou desabilitados, aplicativos cliente, como o módulo do Active Directory para Windows PowerShell ou o Centro Administrativo do Active Directory, não poderão acessar nem gerenciar instâncias do serviço de diretório em execução neste servidor. Para obter mais informações, consulte [que há de novo no AD DS: Serviços da Web do Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) na biblioteca técnica do Windows Server.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>Para configurar o serviço de Serviços Web do Active Directory para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com o botão direito do mouse no serviço **Serviços Web do Active Directory** e clique em **Propriedades**.  
  
3.  Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>O serviço Cliente DHCP deve ser configurado para iniciar automaticamente por padrão  
 **Problema:**  O serviço Cliente DHCP não está configurado para iniciar automaticamente.  
  
 **Impacto:**  O serviço Cliente DHCP não será iniciado automaticamente quando o servidor for iniciado. Se esse serviço for interrompido, os computadores cliente não poderão receber um endereço IP do servidor.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>Para configurar o serviço Cliente DHCP para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Cliente DHCP** e clique em **Propriedades**.  
  
3.  Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>O Serviço de Administração do IIS deve ser configurado para iniciar automaticamente por padrão  
 **Problema:** O Serviço de Administração do IIS não está configurado para iniciar automaticamente.  
  
 **Impacto:** O Serviço de Administração do IIS não será iniciado automaticamente quando o servidor for iniciado. Se esse serviço for interrompido, talvez você não consiga acessar os sites em execução no servidor, como Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>Para configurar o Serviço de Administração do IIS para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse em **Serviço de Administração do IIS** e clique em **Propriedades**.  
  
3.  Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>O Serviço de Publicação na World Wide Web deve ser configurado para iniciar automaticamente por padrão  
 **Problema:**  O Serviço de Publicação na World Wide Web não está configurado para iniciar automaticamente.  
  
 **Impacto:**  O Serviço de Publicação na World Wide Web talvez não seja iniciado automaticamente quando o servidor for iniciado. Se esse serviço for interrompido, talvez você não consiga acessar os sites em execução no servidor, como Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>Para configurar o Serviço de Publicação na World Wide Web para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com o botão direito do mouse em **Serviço de Publicação na World Wide Web** e clique em **Propriedades**.  
  
3.  Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>O serviço Registro Remoto deve ser configurado para iniciar automaticamente por padrão  
 **Problema:**  O serviço Registro Remoto não está configurado para iniciar automaticamente.  
  
 **Impacto:**  
  
 O serviço Registro Remoto talvez não seja iniciado automaticamente quando o servidor for iniciado. Se esse serviço for interrompido, você pode não ser capaz de executar algumas operações de rede remotamente.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>Para configurar o serviço Registro Remoto para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Registro Remoto** e clique em **Propriedades**.  
  
3.  Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>O serviço Gateway de Área de Trabalho Remota deve ser configurado para iniciar automaticamente por padrão  
 **Problema:**  O serviço Gateway de Área de Trabalho Remota não está configurado para iniciar automaticamente.  
  
 **Impacto:**  Se esse serviço for interrompido, talvez os usuários não consigam acessar computadores usando o Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>Para configurar o serviço Gateway de Área de Trabalho Remota para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Gateway de Área de Trabalho Remota** e clique em **Propriedades**.  
  
3.  Na guia **Geral**, altere o **Tipo de inicialização** para **Automático (Atraso na Inicialização)** e clique em **OK**.  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>O serviço Horário do Windows deve ser configurado para iniciar automaticamente por padrão  
 **Problema:**  O serviço Horário do Windows não está configurado para iniciar automaticamente.  
  
 **Impacto:**  Se esse serviço for interrompido, a sincronização de data e hora não ficará disponível.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>Para configurar o serviço Horário do Windows para iniciar automaticamente  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Horário do Windows** e clique em **Propriedades**.  
  
3.  Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>O serviço MSDTC (Coordenador de Transações Distribuídas) deve ser iniciado  
 **Problema:**  O serviço MSDTC não está em execução no servidor.  
  
 **Impacto:**  Se esse serviço for interrompido, algumas funções do SQL Server ou COM podem falhar. Como resultado, os aplicativos que usam funções do Microsoft SQL Server ou COM talvez não funcionem corretamente.  
  
 **Resolução:**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Para iniciar o serviço Coordenador de Transações Distribuídas  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com o botão direito do mouse no serviço **Coordenador de Transações Distribuídas** e clique em **Iniciar**.  
  
### <a name="the-netlogon-service-should-be-started"></a>O serviço Netlogon deve ser iniciado  
 **Problema:**  O serviço Netlogon não está em execução no servidor.  
  
 **Impacto:**  Se esse serviço não for iniciado, o servidor pode não autenticar usuários e serviços.  
  
 **Resolução:**  
  
##### <a name="to-start-the-netlogon-service"></a>Para iniciar o serviço Netlogon  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Netlogon** e clique em **Iniciar**.  
  
### <a name="the-dns-client-service-should-be-started"></a>O serviço Cliente DNS deve ser iniciado  
 **Problema:**  O serviço Cliente DNS não está em execução no servidor.  
  
 **Impacto:**  Se esse serviço não for iniciado, talvez o servidor não consiga resolver nomes DNS.  
  
 **Resolução:**  
  
##### <a name="to-start-the-dns-client-service"></a>Para iniciar o serviço Cliente DNS  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Cliente DNS** e clique em **Iniciar**.  
  
### <a name="the-dns-server-service-should-be-started"></a>O serviço Servidor DNS deve ser iniciado  
 **Problema:**  O serviço Servidor DNS não está em execução no servidor.  
  
 **Impacto:**  Se o serviço Servidor DNS não for iniciado, talvez as atualizações de DNS não sejam realizadas.  
  
 **Resolução:**  
  
##### <a name="to-start-the-dns-server-service"></a>Para iniciar o serviço Servidor DNS  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Servidor DNS** e clique em **Iniciar**.  
  
### <a name="active-directory-web-services-is-not-started"></a>Os Serviços Web do Active Directory não foram iniciados  
 **Problema:**  Os Serviços Web do Active Directory não foram iniciados.  
  
 **Impacto:**  Os ADWS (Serviços Web do Active Directory) não foram iniciados. Se os ADWS no servidor forem interrompidos ou desabilitados, aplicativos cliente, como o módulo do Active Directory para Windows PowerShell ou o Centro Administrativo do Active Directory, não poderão acessar nem gerenciar instâncias do serviço de diretório em execução neste servidor. Para obter mais informações, consulte [que há de novo no AD DS: Serviços da Web do Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) na biblioteca técnica do Windows Server.  
  
 **Resolução:**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>Para iniciar o serviço de Serviços Web do Active Directory  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com o botão direito do mouse em **Serviços Web do Active Directory** e clique em **Iniciar**.  
  
### <a name="the-dhcp-client-service-should-be-started"></a>O serviço Cliente DHCP deve ser iniciado  
 **Problema:**  O serviço Cliente DHCP não está em execução no servidor.  
  
 **Impacto:**  Se esse serviço for interrompido, os computadores cliente não poderão receber um endereço IP do servidor.  
  
 **Resolução:**  
  
##### <a name="to-start-the-dhcp-client-service"></a>Para iniciar o serviço Cliente DHCP  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Cliente DHCP** e clique em **Iniciar**.  
  
### <a name="the-iis-admin-service-should-be-started"></a>O Serviço de Administração do IIS deve ser iniciado  
 **Problema:**  O Serviço de Administração do IIS não está em execução no servidor.  
  
 **Impacto:**  Se esse serviço for interrompido, talvez você não consiga acessar os sites em execução no servidor, como Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-start-the-iis-admin-service"></a>Para iniciar o Serviço de Administração do IIS  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse em **Serviço de Administração do IIS** e clique em **Iniciar**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>O Serviço de Publicação na World Wide Web deve ser iniciado  
 **Problema:**  O Serviço de Publicação na World Wide Web não está em execução no servidor.  
  
 **Impacto:**  Se esse serviço for interrompido, talvez você não consiga acessar os sites em execução no servidor, como Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>Para iniciar o Serviço de Publicação na World Wide Web  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com o botão direito do mouse em **Serviço de Publicação na World Wide Web** e clique em **Iniciar**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>O serviço Gateway de Área de Trabalho Remota deve ser iniciado  
 **Problema:**  O serviço Gateway de Área de Trabalho Remota não está em execução no servidor.  
  
 **Impacto:**  Se esse serviço for interrompido, os usuários podem não conseguir acessar computadores usando o Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>Para iniciar o serviço de Gateway de área de trabalho remota  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Gateway de Área de Trabalho Remota** e clique em **Iniciar**.  
  
### <a name="the-windows-time-service-should-be-started"></a>O serviço Horário do Windows deve ser iniciado  
 **Problema:**  O serviço Horário do Windows não está em execução no servidor.  
  
 **Impacto:**  Se esse serviço for interrompido, a sincronização de data e hora ficará indisponível.  
  
 **Resolução:**  
  
##### <a name="to-start-the-windows-time-service"></a>Para iniciar o serviço de tempo do Windows  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Horário do Windows** e clique em **Iniciar**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>A conta de logon do serviço MSDTC (Coordenador de Transações Distribuídas) deve ser NT AUTHORITY\Network Service  
 **Problema:**  A conta de logon padrão do serviço MSDTC (Coordenador de Transações Distribuídas) foi alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões necessárias para funcionar conforme o esperado. Como resultado, aplicativos que usam funções do SQL Server ou COM podem não funcionar corretamente.  
  
 **Resolução:**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>Para alterar a conta de logon para o serviço  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com o botão direito do mouse no serviço **Coordenador de Transações Distribuídas** e em **Propriedades**.  
  
3.  Na guia **Logon**, selecione **Esta conta**, digite **NT AUTHORITY\Network Service** e clique em **OK**.  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>O serviço Netlogon deve usar a conta Sistema Local como sua conta de logon  
 **Problema:**  A conta de logon padrão do serviço Netlogon está alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões necessárias para funcionar conforme o esperado. Como resultado, o servidor pode não autenticar usuários e serviços.  
  
 **Resolução:**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>Para alterar a conta de logon do serviço Netlogon  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Netlogon** e clique em **Propriedades**.  
  
3.  Na guia **Logon**, selecione **Conta Sistema Local**.  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>O serviço Cliente DNS deve usar a conta NT AUTHORITY\Network Service como sua conta de logon  
 **Problema:**  A conta de logon padrão do serviço Cliente DNS está alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões necessárias para funcionar conforme o esperado. Como resultado, o servidor pode ser incapaz de resolver nomes DNS.  
  
 **Resolução:**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>Para alterar a conta de logon do serviço Cliente DNS  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Cliente DNS** e clique em **Propriedades**.  
  
3.  Na guia **Logon**, selecione **Esta conta** e digite **NT AUTHORITY\Network Service**.  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>O serviço servidor DNS deve usar a conta Sistema Local como sua conta de logon  
 **Problema:**  A conta de logon padrão do serviço Servidor DNS está alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões necessárias para funcionar conforme o esperado. Como resultado, as atualizações de DNS podem não ocorrer.  
  
 **Resolução:**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>Para alterar a conta de logon do serviço Servidor DNS  
  
1.  Abra o services.msc no servidor  
  
2.  Clique com botão direito do mouse no serviço **Servidor DNS** e clique em **Propriedades**.  
  
3.  Na guia **Logon**, selecione **Conta Sistema Local**.  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Os Serviços Web do Active Directory não são a conta de logon padrão  
 **Problema:**  Os Serviços Web do Active Directory não são a conta de logon padrão. Por padrão, a conta de logon é definida como **Conta Sistema Local**.  
  
 **Impacto:**  Os ADWS (Serviços Web do Active Directory) não foram iniciados. Se os ADWS no servidor forem interrompidos ou desabilitados, aplicativos cliente, como o módulo do Active Directory para Windows PowerShell ou o Centro Administrativo do Active Directory, não poderão acessar nem gerenciar instâncias do serviço de diretório em execução neste servidor. Para obter mais informações, consulte [que há de novo no AD DS: Serviços da Web do Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) na biblioteca técnica do Windows Server.  
  
 **Resolução:**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>Para alterar a conta de logon dos Serviços Web do Active Directory  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com o botão direito do mouse em **Serviços Web do Active Directory** e clique em **Propriedades**.  
  
3.  Altere o **Tipo de inicialização** para **Automático** e clique em **OK**.  
  
4.  Nas **Propriedades** dos Serviços Web do Active Directory, clique na guia **Logon**.  
  
5.  Selecione a opção **Conta Sistema Local** e clique em **OK**.  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>O serviço Windows Update deve usar a conta Sistema Local como sua conta de logon  
 **Problema:**  A conta de logon padrão do serviço de Atualizações Automáticas está alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões necessárias para funcionar conforme o esperado. Como resultado, o servidor pode não receber atualizações automáticas.  
  
 **Resolução:**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>Para alterar a conta de logon do serviço Windows Update  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Windows Update** e clique em Propriedades.  
  
3.  Na guia **Logon**, selecione **Conta Sistema Local**.  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>O serviço Cliente DHCP deve usar a conta NT AUTHORITY\LocalService como sua conta de logon  
 **Problema:**  A conta de logon padrão do serviço Cliente DHCP está alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões necessárias para funcionar conforme o esperado. Como resultado, o computador cliente não receberá endereços IP do servidor.  
  
 **Resolução:**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>Para alterar a conta de logon do serviço Cliente DHCP  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Cliente DHCP** e clique em **Propriedades**.  
  
3.  Na guia **Logon**, selecione **Esta conta** e digite **NT AUTHORITY\Local Service**.  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>O Serviço de Administração do IIS deve usar a conta Sistema Local como sua conta de logon  
 **Problema:**  A conta de logon padrão do Serviço de Administração do IIS está alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões necessárias para funcionar conforme o esperado. Como resultado, você pode não ser capaz de acessar os sites em execução no servidor, como Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-change-the-service-logon-account"></a>Para alterar a conta de logon do serviço  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse em **Serviço de Administração do IIS** e clique em **Propriedades**.  
  
3.  Na guia **Logon**, selecione **Conta Sistema Local**.  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>O Serviço de Publicação na World Wide Web deve usar a conta Sistema Local como sua conta de logon  
 **Problema:**  A conta de logon padrão do Serviço de Publicação na World Wide Web está alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões necessárias para funcionar conforme o esperado. Como resultado, você pode não ser capaz de acessar os sites em execução no servidor, como Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>Para alterar a conta de logon do Serviço de Publicação na World Wide Web  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com o botão direito do mouse em **Serviço de Publicação na World Wide Web** e clique em **Propriedades**.  
  
3.  Na guia **Logon**, selecione **Conta Sistema Local**.  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>O serviço Gateway de Área de Trabalho Remota deve usar a conta NT AUTHORITY\Network Service como sua conta de logon  
 **Problema:**  A conta de logon padrão do serviço Gateway de Área de Trabalho Remota está alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões adequadas para funcionar conforme o esperado. Como resultado, os usuários podem não ser capazes de acessar computadores usando o Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>Para alterar a conta de logon do serviço Gateway de Área de Trabalho Remota  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Gateway de Área de Trabalho Remota** e clique em **Propriedades**.  
  
3.  Na guia **Logon**, selecione **Esta conta** e digite **NT AUTHORITY\Network Service**.  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>O serviço Horário do Windows deve usar a conta NT AUTHORITY\Network Service como sua conta de logon  
 **Problema:**  A conta de logon padrão do serviço Horário do Windows está alterada.  
  
 **Impacto:**  O serviço pode não ter as permissões adequadas para funcionar conforme o esperado. Como resultado, a sincronização de data e hora pode estar indisponível.  
  
 **Resolução:**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>Para alterar a conta de logon do serviço Horário do Windows  
  
1.  Abra o services.msc no servidor.  
  
2.  Clique com botão direito do mouse no serviço **Horário do Windows** e clique em **Propriedades**.  
  
3.  Na guia **Logon**, selecione **Esta conta** e digite **NT AUTHORITY\Local Service**.  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>O grupo Administradores interno não tem o direito de fazer logon como um trabalho em lotes  
 **Problema:**  O grupo Administradores interno não tem o direito de fazer logon como um trabalho em lotes.  
  
 **Impacto:**  Se o Administrador cria um alerta e o configura para ser executado quando o Administrador não tiver feito logon, o alerta falhará com um código de erro de 2147943785.  
  
 **Resolução:**  Para obter informações sobre como dar os administradores internos grupo permissão para fazer logon como um trabalho em lotes, consulte [concederá o grupo de administradores interno o direito de fazer logon como um trabalho em lotes](https://technet.microsoft.com/library/jj635076) (https://technet.microsoft.com/library/jj635076).  
  
### <a name="the-windows-firewall-is-turned-off"></a>O Firewall do Windows está desativado  
 **Problema:**  O Firewall do Windows está desativado. O valor padrão é ativado.  
  
 **Impacto:**  Dependendo de suas configurações de firewall, o Firewall do Windows pode ajudar a proteger seu servidor e sua rede contra atividades maliciosas bloqueando a passagem de algumas informações pelo servidor.  
  
 **Resolução:**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>Para ativar o Firewall do Windows no servidor  
  
1.  Abra o Painel de Controle no servidor.  
  
2.  No painel de Controle, clique em **Sistema e Segurança** e clique em **Firewall do Windows**.  
  
3.  Em Firewall do Windows, clique em **Ativar ou desativar o Firewall do Windows**, selecione a opção **Ativar o Firewall do Windows** e clique em **OK**.  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>O adaptador de rede interno não está configurado para registrar o endereço IP no DNS  
 **Problema:**  O adaptador de rede interno não está configurado para registrar seu endereço IP no DNS.  
  
 **Impacto:**  Se o endereço IP do adaptador de rede interno não estiver registrado no DNS, pode não ser possível acessar o servidor usando o nome do computador servidor.  
  
 **Resolução:**  Verifique se o adaptador de rede interno está configurado para registrar no DNS.  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS: os valores das chaves do Registro do DNS ForwardingTimeout e RecursionTimeout são idênticos  
 **Problema:**  O valor da chave do Registro do DNS ForwardingTimeout não deve ser igual ao valor da chave do Registro RecursionTimeout.  
  
 **Impacto:**  Talvez você não consiga acessar os recursos da Internet por nome.  
  
 **Resolução:**  Defina o valor da chave do Registro RecursionTimeout para ser maior do que o valor da chave do Registro ForwardingTimeout, localizada no Registro em HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters.  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>A zona DNS direita de seu domínio do Active Directory não permite atualizações seguras  
 **Problema:**  Você deve configurar a zona de pesquisa direta para permitir somente atualizações dinâmicas seguras.  
  
 **Impacto:**  Quando você habilita atualizações dinâmicas seguras, apenas hosts e usuários autorizados podem fazer alterações nos registros.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>Para configurar a zona de pesquisa direta para seu domínio do Active Directory  
  
1.  Abra o dnsmgmt.msc no servidor.  
  
2.  Clique com o botão direito do mouse na zona de pesquisa direta do seu domínio do Active Directory e clique em **Propriedades**.  
  
3.  Na lista suspensa **Atualizações dinâmicas**, selecione **Apenas seguras** e clique em **OK**.  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>A zona DNS direta não permite atualizações seguras  
 **Problema:**  Você deve configurar a zona de pesquisa direta para a zona _msdcs.* para permitir somente atualizações dinâmicas seguras.  
  
 **Impacto:**  Quando você habilita atualizações dinâmicas seguras, apenas hosts e usuários autorizados podem fazer alterações nos registros na zona msdcs.*.  
  
 **Resolução:**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>Para permitir atualizações seguras na zona _ msdcs  
  
1.  Abra o dnsmgmt.msc no servidor.  
  
2.  Clique com o botão direito do mouse na zona de pesquisa direta da zona _msdcs e clique em **Propriedades**.  
  
3.  Na lista suspensa **Atualizações dinâmicas**, selecione **Apenas seguras** e clique em **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>A Configuração de Segurança Aprimorada do Internet Explorer não está habilitada  
 **Problema:**  Internet Explorer Enhanced Security Configuration (IE ESC) atualmente não está habilitado para o grupo Administradores.  
  
 **Impacto:**  Se a Configuração de Segurança Aprimorada do Internet Explorer não estiver habilitada para o grupo Administradores, o servidor e o Internet Explorer terão uma exposição maior aos ataques maliciosos que podem ocorrer por meio de scripts de aplicativos e conteúdo da Web.  
  
 **Resolução:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Para habilitar a Configuração de Segurança Aprimorada do Internet Explorer  
  
1.  Abra o **Gerenciador de Servidores** no servidor e clique em **Servidor Local**.  
  
2.  No painel **Propriedades**, altere a configuração de **Configuração de Segurança Aprimorada do IE** para **Ativado** e clique em **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>A Configuração de Segurança Aprimorada do Internet Explorer não está habilitada  
 **Problema:**  Internet Explorer Enhanced Security Configuration (IE ESC) atualmente não está habilitado para o grupo de usuários.  
  
 **Impacto:**  Se a Configuração de Segurança Aprimorada do Internet Explorer não estiver habilitada para o grupo Usuários, o servidor e o Internet Explorer têm uma exposição maior aos ataques maliciosos que podem ocorrer por meio de scripts de aplicativos e conteúdo da Web.  
  
 **Resolução:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Para habilitar a Configuração de Segurança Aprimorada do Internet Explorer  
  
1.  Abra o **Gerenciador de Servidores** e clique em **Servidor Local**.  
  
2.  No painel **Propriedades**, altere a configuração de **Configuração de Segurança Aprimorada do IE** para **Ativado** e clique em **OK**.  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>O servidor de origem permanece em Sites e Serviços do Active Directory  
 **Problema:**  O servidor de origem que está executando o Windows Small Business Server ainda existe em Sites e Serviços do Active Directory no Default-First-Site-Name.  
  
 **Impacto:**  Se o servidor de origem permanece em serviços e Sites do Active Directory, os computadores cliente podem ter problema de conectividade: s.  
  
 **Resolução:**  Você deve rebaixar o servidor de origem, removê-lo do domínio e, em seguida, excluir o servidor de origem dos Sites e Serviços do Active Directory e dos Usuários e Computadores do Active Directory.  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>O servidor de origem permanece na UO do Computador SBS  
 **Problema:**  O servidor de origem que está executando o Windows Small Business Server ainda existe nos Usuários e Computadores do Active Directory.  
  
 **Impacto:**  Se o servidor de origem permanece no Active Directory Users and Computers, computadores cliente podem ter o problema de conectividade: s.  
  
 **Resolução:**  Você deve rebaixar o servidor de origem, removê-lo do domínio e, em seguida, excluir o servidor de origem dos Sites e Serviços do Active Directory e dos Usuários e Computadores do Active Directory.  
  
### <a name="a-group-policy-is-missing"></a>Uma política de grupo está ausente  
 **Problema:**  A política de grupo da Política de Domínio Padrão está ausente.  
  
 **Impacto:**  A Política de Domínio Padrão é necessária para as funções de domínio adequadas.  
  
 **Resolução:**  
  
##### <a name="to-restore-a-missing-group-policy"></a>Para restaurar uma política de grupo ausente  
  
1.  Abra o gpmc.msc no servidor.  
  
2.  No Gerenciador de Política de Grupo, expanda a floresta de domínio e pesquise a árvore de console do objeto de política de grupo **Política de Domínio Padrão**.  
  
3.  Se a política não aparecer na árvore, restaure-a de um backup de estado do sistema.  
  
### <a name="no-dns-name-server-resource-records"></a>Nenhum registro de recurso de servidor de nomes DNS  
 **Problema:**  Não há registros de recursos de NS (servidor de nomes) DNS na zona de pesquisa direta para o servidor.  
  
 **Impacto:**  Se não houver registros de recursos de NS (servidor de nomes) DNS na zona de pesquisa direta para o domínio do Active Directory, talvez os usuários não consigam acessar os recursos na rede ou na Internet.  
  
 **Resolução:**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>Para restaurar os registros de recurso de servidor de nomes DNS  
  
1.  Abra o dnsmgmt.msc no servidor.  
  
2.  No Gerenciador DNS, clique com o botão direito do mouse na zona de pesquisa direta para o domínio do Active Directory e clique em **Propriedades**.  
  
3.  Na guia **Servidores de Nomes**, verifique se as configurações estão corretas.  
  
4.  Faça as alterações necessárias e clique em **OK** para salvar as configurações.  
  
### <a name="no-dns-name-server-records"></a>Nenhum registro de servidor de nomes DNS  
 **Problema:**  Não há registros de recursos de NS (servidor de nomes) DNS na zona _msdcs para o servidor (por exemplo: _msdcs.contoso.local).  
  
 **Impacto:**  Se não houver registros de recursos de NS (servidor de nomes) DNS na zona _msdcs para o domínio do Active Directory, os usuários podem não ser capazes de acessar os recursos na rede ou na Internet.  
  
 **Resolução:**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>Para restaurar os registros de servidor de nomes DNS  
  
1.  Abra o dnsmgmt.msc no servidor.  
  
2.  No Gerenciador DNS, clique com o botão direito do mouse na zona de pesquisa direta da zona _msdcs e clique em **Propriedades**.  
  
3.  Na guia **Servidores de Nomes**, verifique se as configurações estão corretas.  
  
4.  Faça as alterações necessárias e clique em **OK** para salvar as configurações.  
  
### <a name="no-dns-name-server-records"></a>Nenhum registro de servidor de nomes DNS  
 **Problema:**  Não há registros de recursos de NS (servidor de nomes) DNS para a zona de pesquisa direta _msdcs delegada.  
  
 **Impacto:**  Se não houver registros de recursos de NS (servidor de nomes) DNS para a zona de pesquisa direta _msdcs, o serviço do Servidor DNS não poderá resolver os registros de recursos DNS para o domínio e não inicializará.  
  
 **Resolução:**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>Para reconfigurar os registros de servidor de nomes DNS  
  
1.  Abra o dnsmgmt.msc no servidor.  
  
2.  No Gerenciador DNS, expanda o nome do servidor e expanda **Zonas de Pesquisa Direta**.  
  
3.  Clique na zona de pesquisa direita para seu domínio do Active Directory (por exemplo: contoso.local).  
  
4.  A zona _msdcs delegada aparece como uma pasta acinzentada. Clique com o botão direito do mouse na zona _msdcs e clique em **Propriedades**.  
  
5.  Na guia **Servidores de Nomes**, verifique se as configurações estão corretas.  
  
6.  Faça as alterações necessárias e clique em **OK** para salvar as configurações.  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>O grupo Usuários Autenticados não é um membro do grupo de acesso compatível com versões anteriores ao Windows 2000  
 **Problema:**  O grupo Usuários Autenticados não é um membro do grupo de acesso compatível com versões anteriores ao Windows 2000.  
  
 **Impacto:**  Se o grupo Usuários Autenticados internos não for um membro do grupo de acesso compatível com versões anteriores ao Windows 2000, os usuários da rede poderão encontrar erros de "Acesso negado".  
  
 **Resolução:**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>Para adicionar o grupo Usuários Autenticados ao grupo de acesso compatível com versões anteriores ao Windows 2000  
  
1.  Abra o dsa.msc no servidor.  
  
2.  Na pasta **Interno**, clique com o botão direito do mouse em **Acesso compatível com versões anteriores ao Windows 2000** e clique em **Propriedades**.  
  
3.  Clique em **Adicionar**, digite **Usuários Autenticados** e clique em **OK** duas vezes.  
  
### <a name="dns-client-not-configured"></a>Cliente DNS não configurado  
 **Problema:**  O cliente DNS não está configurado para apontar apenas para o endereço IP interno do servidor.  
  
 **Impacto:**  Se o cliente DNS não estiver configurado para apontar apenas para o endereço IP interno do servidor, poderá ocorrer falha na resolução do nome DNS.  
  
 **Resolução:**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>Para configurar o DNS para apontar apenas para o endereço IP interno do servidor s  
  
1.  No computador cliente, abra a página **Propriedades** para a conexão de rede.  
  
2.  Certifique-se de que o DNS esteja configurado para apontar apenas para o endereço IP interno do servidor.  
  
### <a name="default-application-pool-value-changed"></a>Valor do Pool de Aplicativos Padrão alterado  
 **Problema:**  O número de Máximo de Processos do Operador para o Pool de aplicativos DefaultAppPool não está definido como o valor padrão de 1.  
  
 **Impacto:**  Talvez os usuários não consigam se conectar aos serviços baseados na Web do Windows Small Business Server.  
  
 **Resolução:**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>Para redefinir o Máximo de Processos do Operador para o pool de aplicativos padrão  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e clique em **Pools de Aplicativos**.  
  
3.  Em **Pools de Aplicativos**, clique com o botão direito do mouse em **DefaultAppPool** e clique em **Configurações Avançadas**.  
  
4.  Em **Configurações Avançadas**, altere o valor de **Máximo de Processos do Operador** para 1 e clique em **OK**.  
  
5.  Feche as **Configurações Avançadas**, clique com o botão direito do mouse em **DefaultAppPool** e depois pare e reinicie o pool de aplicativos.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>O pool de aplicativos para o Acesso Remoto via Web não usa a conta padrão  
 **Problema:**  O pool de aplicativos RemoteAppPool não está sendo executado com a conta padrão.  
  
 **Impacto:**  Talvez os usuários da rede não consigam acessar o site de Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>Para redefinir o pool de aplicativos remotos para usar a conta padrão  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e clique em **Pools de Aplicativos**.  
  
3.  Em **Pools de Aplicativos**, clique com o botão direito do mouse em **RemoteAppPool** e clique em **Configurações Avançadas**.  
  
4.  Em **Configurações Avançadas**, altere a **Identidade** para **NetworkService** e clique em **OK**.  
  
5.  Feche as **Configurações Avançadas**, clique com o botão direito do mouse em **RemoteAppPool** e depois pare e reinicie o pool de aplicativos.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>O pool de aplicativos para o Acesso Remoto via Web não usa a versão do .NET Framework padrão  
 **Problema:**  O pool de aplicativos RemoteAppPool não está sendo executado com a versão padrão do Microsoft .NET Framework.  
  
 **Impacto:**  Talvez os usuários da rede não consigam acessar o site de Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>Para alterar a versão do .NET Framework utilizada pelo RemoteAppPool  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e clique em **Pools de Aplicativos**.  
  
3.  Em **Pools de Aplicativos**, clique com o botão direito do mouse em **RemoteAppPool** e clique em **Configurações Avançadas**.  
  
4.  Em **Configurações Avançadas**, altere a **Versão do .NET Framework** para v4.0 e clique em **OK**.  
  
5.  Feche as **Configurações Avançadas**, clique com o botão direito do mouse em **RemoteAppPool** e depois pare e reinicie o pool de aplicativos.  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>O arquivo RemoteAccess.log tem mais que 1 GB de tamanho  
 **Problema:**  Se o tamanho do arquivo Remoteaccess.log exceder 1 GB, poderão ocorrer erros de espaço em disco insuficiente na unidade do sistema.  
  
 **Impacto:**  Se o arquivo remoteaccess log é muito grande, isso poderá causar problema de espaço livre: s na unidade c.  
  
 **Resolução:**  Depois de fazer backup do servidor, você pode excluir o arquivo Remoteaccess.log, que está localizado na pasta %ProgramData%\Microsoft\Windows Server\Logs\WebApps.  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>O diretório do log do site padrão tem mais que 1 GB de tamanho  
 **Problema:**  Se o tamanho da pasta de log do site padrão exceder 1 GB, você pode enfrentar erros de espaço em disco insuficiente na unidade do sistema.  
  
 **Impacto:**  Se a pasta de log do site padrão for muito grande, isso poderá causar problema de espaço livre: s na unidade c:  
  
 **Resolução:**  Depois de fazer backup do servidor e, enquanto o site padrão é interrompido, você pode excluir os arquivos de log na pasta C:\inetpub\logs\LogFiles\W3SVC1. Depois, inicie o site padrão.  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>Nenhuma associação para o SSL em todos os endereços IP  
 **Problema:**  Não há nenhuma associação para o protocolo SSL em todos os endereços IP no servidor.  
  
 **Impacto:**  Se o SSL não estiver associado a todos os endereços IP no servidor, alguns sites não estarão disponíveis para os usuários.  
  
 **Resolução:**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>Para associar o SSL a todos os endereços IP no servidor  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador dos IIS, no painel **Conexões**, expanda seu servidor, expanda **Sites**, clique com o botão direito do mouse em **Site Padrão** e clique em **Editar Associações**.  
  
3.  Em **Associações do Site**, clique em **Adicionar** e selecione as seguintes configurações:  
  
    -   **Type** = **https**  
  
    -   **Endereço IP** = **todos não atribuídos**  
  
    -   **Porta** = 443  
  
4.  Selecione um certificado SSL e clique em **OK** para salvar suas alterações.  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>Nenhuma associação para SSL no site padrão  
 **Problema:**  Não há nenhuma associação para SSL no site padrão.  
  
 **Impacto:**  Se o SSL não estiver associado ao site padrão, alguns sites poderão não estar disponíveis aos usuários.  
  
 **Resolução:**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>Para associar o SSL ao site padrão  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador dos IIS, no painel **Conexões**, expanda seu servidor, expanda **Sites**, clique com o botão direito do mouse em **Site Padrão** e clique em **Editar Associações**.  
  
3.  Em **Associações do Site**, clique em **Adicionar** e selecione as seguintes opções:  
  
    -   **Type** = **https**  
  
    -   **Endereço IP** = **todos não atribuídos**  
  
    -   **Porta** = 443  
  
    > [!NOTE]
    >  Se existir uma associação de HTTPS para a porta 443 para um endereço IP específico, altere o atributo **Endereço IP** dessa associação para **Todos os Não Atribuídos**. Há uma exceção para o endereço IP 127.0.0.1. Não altere a associação para 127.0.0.1.  
  
4.  Selecione um certificado SSL e clique em **OK** para salvar suas alterações.  
  
### <a name="a-certificate-expires-within-30-days"></a>Um certificado expira em 30 dias  
 **Problema:**  O certificado do seu servidor expirará em 30 dias.  
  
 **Impacto:**  O servidor não pode usar um certificado expirado. Se o certificado expirar, talvez os usuários não consigam usar as funções de Acesso em Qualquer Local.  
  
 **Resolução:**  Para impedir que o certificado expire, renove o certificado com a Autoridade de Certificação Confiável.  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>A entidade do certificado não corresponde ao nome configurado pelo assistente de Nome de Domínio  
 **Problema:**  A entidade do certificado não corresponde ao nome que foi configurado pelo assistente de Nome de Domínio.  
  
 **Impacto:**  Se a entidade do certificado não corresponder ao nome que foi configurado pelo assistente de Nome de Domínio, alguns sites não serão inicializados. Outros sites exibirão o erro "Há um problema com o certificado de segurança deste site."  
  
 **Resolução:**  Para resolver esse problema:, execute o Assistente de acesso em qualquer lugar de configuração novamente e forneça o nome de domínio correto para o certificado ou comprar um novo certificado que corresponda ao nome de domínio que você deseja usar.  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>Uma ou mais contas de usuários têm nomes CN duplicados  
 **Problema:**  Uma ou mais contas de usuário têm nomes CN duplicados: {0}.  
  
 **Impacto:**  Se as contas de usuário tiverem nomes CN duplicados, talvez os usuários não consigam fazer logon na rede. Além disso, as pesquisas do Active Directory para usuários podem retornar valores incorretos.  
  
 **Resolução:**  Para resolver esse problema:, certifique-se de que as contas de usuário de rede não têm duplicado "CN =" nomes. Para facilitar o processo, considere exportar o conteúdo do Active Directory para um arquivo de texto para revisão. Para obter informações sobre como fazer isso, consulte [usando o LDIFDE para importar e exportar objetos de diretório Active Directory (artigo da Base de dados de Conhecimento 237677)](https://support.microsoft.com/kb/237677) (https://support.microsoft.com/kb/237677).  
  
### <a name="nt-backup-is-installed"></a>O backup do NT está instalado  
 **Problema:**  O programa de backup do Windows NT está instalado no servidor.  
  
 **Impacto:**   Windows Server Essentials usa o Backup do Windows Server. Se o programa de backup do Windows NT também estiver instalado, poderão existir conflitos entre os dois programas de backup. Isso pode fazer com que o processo do Backup do Windows Server falhe. Os conflitos também podem impedir que você use um backup para restaurar o servidor.  
  
 **Resolução:** Para resolver esse problema:, desinstale o programa de Backup do NT do servidor.  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>O IIS não tem a porta 80 (0.0.0.0:80) nem a porta 443 (0.0.0.0:443)  
 **Problema:**  O IIS (Serviços de Informações da Internet) não tem a porta 80 (0.0.0.0:80) nem a porta 443. No momento, essas portas estão associadas por outros aplicativos.  
  
 **Impacto:**   Aplicativos do Windows Server Essentials web exigem o uso de portas 80 e 443 para disponibilizar serviços aos usuários. Se outro processo ou aplicativo já estiver usando a porta 80 ou 443, não é possível executar aplicativos da web do Windows Server Essentials. Se isso ocorrer, o Acesso Remoto via Web e outros aplicativos não estarão disponíveis para os usuários.  
  
 **Resolução:**  Para resolver esse problema:, a desinstalar o aplicativo que já esteja usando a porta 80 ou 443, ou atribuir esse aplicativo para uma porta diferente.  
  
### <a name="the-default-website-is-not-running"></a>O site padrão não está em execução  
 **Problema:**  O site padrão não está em execução no seu ambiente do Windows Server Essentials.  
  
 **Impacto:**   Aplicativos do Windows Server Essentials web exigem o uso do site da Web padrão. Se o site padrão não estiver em execução, o Acesso Remoto via Web e outros aplicativos não estarão disponíveis para os usuários.  
  
 **Resolução:**  
  
##### <a name="to-start-the-default-website"></a>Para iniciar o site padrão  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e clique em **Sites**.  
  
3.  Clique com o botão direito do mouse em **Site Padrão**, aponte para **Gerenciar Site** e clique em **Iniciar**.  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>As permissões de Leitura e Script para o diretório virtual /Remote estão incorretas  
 **Problema:**  As permissões de Leitura e Script não estão atribuídas ao diretório virtual /Remote.  
  
 **Impacto:**  Se as permissões de Leitura e Script para o diretório virtual /Remote estiverem incorretas, os usuários não poderão usar o Acesso Remoto via Web. Quando eles tentam usar o acesso via Web remoto para navegar pela Internet, podem ocorrer o erro "Erro de HTTP 403.1 proibido".  
  
 **Resolução:**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>Para atribuir permissões de Leitura e Script ao diretório /Remote  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e clique em **Sites**.  
  
3.  Expanda **Site Padrão** e expanda **Remoto**.  
  
4.  Em **Exibição de Recursos**, clique duas vezes em **Mapeamentos do Manipulador**.  
  
5.  No painel **Ações**, clique em **Editar Permissões de Recurso**.  
  
6.  Marque as caixas de seleção **Leitura** e **Script** e clique em **OK**.  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>O Redirecionamento HTTP é definido ou herdado no diretório virtual /Remote  
 **Problema:**  O atributo de Redirecionamento HTTP é definido ou herdado inesperadamente no diretório virtual /Remote.  
  
 **Impacto:**  Se o atributo de Redirecionamento HTTP for definido no diretório virtual /Remote, o Local de Trabalho Remoto da Web não funcionará corretamente.  
  
 **Resolução:**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>Para remover o atributo de Redirecionamento HTTP  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e clique em **Sites**.  
  
3.  Expanda **Site Padrão** e expanda **Remoto**.  
  
4.  Em **Exibição de Recursos**, clique duas vezes em **Redirecionamento HTTP**.  
  
5.  Desmarque a caixa de seleção **Redirecionar solicitações para este destino** e clique em **Aplicar** no painel **Ações**.  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>Existe um nome de host para a porta 80 no site padrão  
 **Problema:**  Há um nome de host atribuído à porta 80 no site padrão.  
  
 **Impacto:**  Se um nome de host é atribuído à porta 80 no site da Web padrão, você não poderá se conectar a alguns aplicativos de web do Windows Server Essentials. Um nome de host não é obrigatório e não é recomendável nessa situação  
  
 **Resolução:**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>Para limpar a entrada de nome de host para a porta 80 no site padrão  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e clique em **Sites**.  
  
3.  Em **Exibição de Recursos**, clique com botão direito em **Site Padrão** e clique em **Associações**.  
  
4.  Em **Associações do Site**, selecione a configuração **http para a porta 80** e clique em **Editar**.  
  
5.  Em **Editar Ligação do Site**, limpe a entrada **Nome de host** e clique e, **OK**.  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>O backup falha devido a uma partição oculta  
 **Problema:**  Uma partição não NTFS é agendada para backup pelo Backup do Windows Server.  
  
 **Impacto:**  O Backup do Windows Server pode fazer backup apenas de partições que estão formatadas como NTFS.  
  
 **Resolução:**  Não configure o Backup do Windows Server para fazer backup de partições não NTFS. Para obter mais informações, consulte [identificações de evento 12290 e 16387 são registradas quando o backup do estado do sistema Falha em um computador baseado no Windows Server 2008 (artigo da Base de dados de Conhecimento 968128)](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128).  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>O backup mais recente não teve êxito  
 **Problema:**  A tentativa de backup mais recente não foi concluída com êxito.  
  
 **Impacto:**  O status do backup para o sistema não está correto.  
  
 **Resolução:**  Examine os logs de eventos e logs de backup para encontrar os erros que ocorreram durante o backup mais recente.  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>O tipo de inicialização do Serviço de Replicação de Arquivos não está definido como Automático  
 **Problema:**  O FRS (Serviço de Replicação de Arquivos) talvez não seja iniciado se o tipo de inicialização não for definido com o valor padrão Automático.  
  
 **Impacto:**  Se o Serviço de Replicação de Arquivos não estiver em execução, o controlador de domínio poderá parar de anunciar seus serviços. Isso pode levar a outros problemas, como erros de logon e de Política de Grupo.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>Para configurar o Serviço de Replicação de Arquivos para inicialização automática  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Replicação de Arquivos**.  
  
3.  Para **Tipo de inicialização**, selecione **Automática** e clique em **Aplicar**.  
  
### <a name="the-file-replication-service-is-not-running"></a>O Serviço de Replicação de Arquivos não está em execução  
 **Problema:**  O Serviço de Replicação de Arquivos não está em execução.  
  
 **Impacto:**  Se o Serviço de Replicação de Arquivos não estiver em execução, o controlador de domínio poderá parar de anunciar seus serviços. Esse comportamento pode levar a outros problemas, como erros de logon e de Política de Grupo.  
  
 **Resolução:**  
  
##### <a name="to-start-the-file-replication-service"></a>Para iniciar o Serviço de Replicação de Arquivos  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Serviço de Replicação de Arquivos**.  
  
3.  Clique em **Iniciar**.  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>A conta de logon para o Serviço de Replicação de Arquivos não está configurada para usar a conta Sistema Local  
 **Problema:**  O Serviço de Replicação de Arquivos não está configurado para usar a conta Sistema Local como a conta de logon padrão.  
  
 **Impacto:**  Se o Serviço de Replicação de Arquivos não usar Sistema Local como a conta de logon padrão, você poderá encontrar erros relacionados a permissões. Esses erros podem disparar outros erros e, eventualmente, podem fazer com que o controlador de domínio pare de anunciar seus serviços.  
  
 **Resolução:**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>Para configurar Sistema Local como a conta de logon padrão para Replicação de Arquivos  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Replicação de Arquivos**.  
  
3.  Na página **Propriedades do Serviço**, clique na guia **Logon**.  
  
4.  Selecione a opção **Conta Sistema Local** e clique em **Aplicar**.  
  
5.  Reinicie o serviço.  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>O tipo de inicialização do serviço de Replicação do DFS não está definido como Automático  
 **Problema:**  O serviço de Replicação do DFS talvez não seja iniciado se o tipo de inicialização não for definido com o valor padrão Automático.  
  
 **Impacto:**  Se o serviço de Replicação do DFS não estiver em execução, o controlador de domínio poderá parar de anunciar seus serviços. Isso pode levar a outros problemas, como erros de logon e de Política de Grupo.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>Para configurar o serviço de Replicação do DFS para inicialização automática  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Replicação do DFS**.  
  
3.  Para **Tipo de inicialização**, selecione **Automática** e clique em **Aplicar**.  
  
### <a name="the-dfs-replication-service-is-not-running"></a>O serviço de Replicação do DFS não está em execução  
 **Problema:**  O serviço de Replicação do DFS não está em execução no momento.  
  
 **Impacto:**  Se o serviço de Replicação do DFS não estiver em execução, o controlador de domínio poderá parar de anunciar seus serviços. Esse comportamento pode levar a outros problemas, como erros de logon e de Política de Grupo.  
  
 **Resolução:**  
  
##### <a name="to-start-the-dfs-replication-service"></a>Para iniciar o serviço de Replicação do DFS  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Replicação do DFS**.  
  
3.  Clique em **Iniciar**.  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>O serviço de Replicação do DFS não está configurado para usar a conta Sistema Local  
 **Problema:**  O serviço de Replicação do DFS não está configurado para usar a conta Sistema Local como a conta de logon padrão.  
  
 **Impacto:**  Se o serviço de Replicação do DFS não usar Sistema Local como a conta de logon padrão, você poderá encontrar erros relacionados a permissões. Esses erros podem disparar outros erros e, eventualmente, podem fazer com que o controlador de domínio pare de anunciar seus serviços.  
  
 **Resolução:**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>Para configurar a Replicação do DFS para usar Sistema Local como a conta de logon padrão  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Replicação do DFS**.  
  
3.  Na página **Propriedades do Serviço**, clique na guia **Logon**.  
  
4.  Selecione a opção **Conta Sistema Local** e clique em **Aplicar**.  
  
5.  Reinicie o serviço.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>O Serviço de Integração do Windows Server Office 365 não está configurado para usar a conta Sistema Local  
 **Problema:**  O Serviço de Integração do Windows Server Office 365 não está configurado para usar a conta Sistema Local como a conta de logon padrão.  
  
 **Impacto:**  Se o Serviço de Integração do Windows Server Office 365 não usar o sistema Local como a conta de logon padrão, alguns recursos do Office 365 talvez não funcionem corretamente. Você também pode encontrar erros relacionados a permissões.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>Para configurar o Serviço de Integração do Windows Server Office 365 para usar Sistema Local como a conta de logon padrão  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Serviço de Integração do Windows Server Office 365**.  
  
3.  Na página **Propriedades do Serviço**, clique na guia **Logon**.  
  
4.  Selecione a opção **Conta Sistema Local** e clique em **Aplicar**.  
  
5.  Reinicie o serviço.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>O Serviço de Integração do Windows Server Office 365 não está em execução  
 **Problema:**  O Serviço de Integração do Windows Server Office 365 não está em execução no momento.  
  
 **Impacto:**  Se o Serviço de Integração do Windows Server Office 365 não estiver em execução, os recursos baseados em nuvem do Office 365 não estarão disponíveis.  
  
 **Resolução:**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>Para iniciar o Serviço de Integração do Windows Server Office 365  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Serviço de Integração do Windows Server Office 365**.  
  
3.  Clique em **Iniciar**.  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>O tipo de inicialização para o Serviço de Integração do Windows Server Office 365 não está configurado como Automático  
 **Problema:**  O Serviço de Integração do Windows Server Office 365 talvez não seja iniciado se o tipo de inicialização não for definido com o valor padrão Automático.  
  
 **Impacto:**  Se o Serviço de Integração do Windows Server Office 365 não estiver em execução, os recursos baseados em nuvem do Office 365 não estarão disponíveis.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>Para configurar o Serviço de Integração do Windows Server Office 365 para inicialização automática  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Serviço de Integração do Windows Server Office 365**.  
  
3.  Para **Tipo de inicialização**, selecione **Automática** e clique em **Aplicar**.  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>Um valor do Registro está ausente ou definido incorretamente  
 **Problema:**  Uma chave do Registro em HKEY_LOCAL_MACHINE \Software\Microsoft\Rpc\RpcProxy contém valores incorretos ou não existe.  
  
 **Impacto:**  Se a chave do Registro RPCProxy estiver configurada incorretamente, você poderá receber uma mensagem de erro semelhante à seguinte: "O computador não pode se conectar ao computador remoto porque o servidor de Gateway de Área de Trabalho Remota está temporariamente indisponível. Tente se reconectar mais tarde ou entre em contato com o administrador da rede para obter assistência."  
  
 **Resolução:**  
  
##### <a name="to-correct-the-registry-setting"></a>Para corrigir a configuração do Registro  
  
1.  Abra o Editor do Registro.  
  
2.  Navegue até a seguinte chave do Registro:  
  
     HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy  
  
3.  Certifique-se de que a cadeia de caracteres chamada "Site" tem um valor de dados do Site Web padrão:  
  
    -   se o valor dos dados estiver incorreto, modifique a cadeia de caracteres para usar o valor correto.  
  
    -   Se a cadeia de caracteres não existir, crie uma nova cadeia de caracteres chamada "Site" e defina o valor de dados ao Site da Web padrão."  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>O tipo de inicialização para o Serviço de Mecanismo de Backup em Nível de Bloco não está definido como Manual  
 **Problema:**  O Serviço de Mecanismo de Backup em Nível de Bloco não está usando o tipo padrão de inicialização de Manual.  
  
 **Impacto:**  Se o serviço for interrompido, poderá haver falhas em algumas funções do SQL Server ou COM. Esse problema: pode causar falha nos trabalhos de Backup do Windows Server.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>Para configurar o Serviço de Mecanismo de Backup em Nível de Bloco para inicialização manual  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Serviço de Mecanismo de Backup em Nível de Bloco**.  
  
3.  Para **Tipo de inicialização**, selecione **Manual** e clique em **Aplicar**.  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>A conta de logon para o Serviço de Mecanismo de Backup em Nível de Bloco não está configurada para usar a conta Sistema Local  
 **Problema:**  O Serviço de Mecanismo de Backup em Nível de Bloco não está configurado para usar a conta Sistema Local como a conta de logon padrão.  
  
 **Impacto:**  Se o Serviço de Mecanismo de Backup em Nível de Bloco não usar Sistema Local como a conta de logon padrão, você poderá encontrar erros relacionados a permissões. Esses erros podem impedir que os trabalhos do Backup do Windows Server sejam concluídos com êxito.  
  
 **Resolução:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>Para configurar o Serviço de Mecanismo de Backup em Nível de Bloco para usar sistema Local como a conta de logon padrão  
  
1.  Abra o console Serviços.  
  
2.  Na lista de serviços, clique duas vezes em **Serviço de Mecanismo de Backup em Nível de Bloco**.  
  
3.  Na página **Propriedades do Serviço**, clique na guia **Logon**.  
  
4.  Selecione a opção **Conta Sistema Local** e clique em **Aplicar**.  
  
5.  Reinicie o serviço.  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>O nome comum no certificado associado ao site do Serviço Web do Certificado WSS não corresponde ao nome do servidor  
 **Problema:**  Um certificado inválido está associado ao site do Serviço Web do Certificado WSS nos IIS. O nome comum nesse certificado não corresponde ao nome do servidor.  
  
 **Impacto:**  Se o certificado para o serviço de Gateway de Área de Trabalho Remota não estiver configurado corretamente, os usuários não poderão se conectar ao Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>Para configurar um certificado válido para o Serviço Web do Certificado WSS  
  
1.  Abra o Gerenciador dos IIS (Serviços de Informações da Internet) no servidor.  
  
2.  No Gerenciador do IIS, expanda o nome do servidor e clique em **Sites**.  
  
3.  Clique com o botão direito do mouse em **Serviço Web do Certificado WSS** e em **Editar Associações**.  
  
4.  Na **ligações do Site**, clique em **HTTPS**e, em seguida, clique em **editar**.  
  
5.  Em **Editar Ligação do Site**, selecione o certificado que tem o mesmo nome de seu servidor para **Certificado SSL**.  
  
6.  Se mais de uma entrada de certificado tiver o mesmo nome de seu servidor, clique em **Exibir** para determinar qual certificado é válido e selecione o certificado apropriado.  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>Parece haver um problema com a associação do certificado para o serviço de Gateway de Área de Trabalho Remota  
 **Problema:**  O certificado para o serviço de Gateway de Área de Trabalho Remota parece estar associado incorretamente.  
  
 **Impacto:**  Se o certificado para o serviço de Gateway de Área de Trabalho Remota não estiver configurado corretamente, os usuários não podem se conectar ao Acesso Remoto via Web.  
  
 **Resolução:**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>Para corrigir a associação para o serviço de Gateway de Área de Trabalho Remota  
  
-   Abra um prompt de comando como administrador e digite os seguintes comandos:  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     Para obter mais informações, consulte [como gerenciar o serviço de Gateway de área de trabalho remota no Windows Server Essentials (artigo da Base de dados de Conhecimento 2472211)](https://support.microsoft.com/kb/2472211) (https://support.microsoft.com/kb/2472211).
