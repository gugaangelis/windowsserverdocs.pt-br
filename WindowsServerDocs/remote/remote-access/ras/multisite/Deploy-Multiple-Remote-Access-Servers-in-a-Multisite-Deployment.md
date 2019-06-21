---
title: Implantar vários servidores de acesso remoto em uma implantação multissite
description: Este tópico faz parte do guia de implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 607e3f5b2aa7e4c81e507a3d551d3d56e1f0b347
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282590"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>Implantar vários servidores de acesso remoto em uma implantação multissite

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

 Windows Server 2016 e Windows Server 2012 combinam o DirectAccess e o serviço de acesso remoto (RAS) VPN em uma única função de acesso remoto. O Acesso Remoto pode ser implantado em diversos cenários corporativos. Esta visão geral fornece uma introdução ao cenário corporativo para implantar servidores de acesso remoto em uma configuração multissite.  
  
## <a name="BKMK_OVER"></a>Descrição do cenário  
Em uma implantação multissite dois ou mais servidores de acesso remoto ou clusters de servidores são implantados e configurados como pontos de entrada diferentes em um único local ou em locais geograficamente dispersos. Implantação de vários pontos de entrada em um único local permite a redundância do servidor, ou para o alinhamento dos servidores de acesso remoto com a arquitetura de rede existente. Implantação por local geográfico garante o uso eficiente de recursos, como computadores cliente remotos podem se conectar aos recursos de rede interna usando um ponto de entrada mais próximo a eles. O tráfego em uma implantação multissite pode ser distribuído e balanceado com um balanceador de carga global externo.  
  
Uma implantação multissite dá suporte a computadores cliente que executam o Windows 10, Windows 8 ou Windows 7. Computadores cliente que executam o Windows 10 ou Windows 8 automaticamente identificam um ponto de entrada, ou o usuário pode selecionar manualmente um ponto de entrada. A atribuição automática ocorre na seguinte ordem de prioridade:  
  
1.  Use um ponto de entrada selecionado manualmente pelo usuário.  
  
2.  Use um ponto de entrada identificado por um balanceador de carga global externo se um for implantado.  
  
3.  Use o ponto de entrada mais próximo, identificado pelo computador cliente usando um mecanismo de teste automático.  
  
Suporte para clientes que executam o Windows 7 deve ser habilitado manualmente em cada ponto de entrada, e não há suporte para a seleção de um ponto de entrada por esses clientes.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Antes de começar a implantar este cenário, examine esta lista de requisitos importantes:  
  
-   [Implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) deve ser implantado antes de uma implantação multissite.  
  
-   Os clientes do Windows 7 sempre se conectará a um site específico. Eles não poderão se conectar ao site mais próximo com base no local do cliente (ao contrário dos clientes do Windows 10, 8 ou 8.1).  
  
-   Não há suporte para alteração de políticas fora do console de gerenciamento do DirectAccess ou dos cmdlets do PowerShell.  
  
-   Uma infraestrutura de chave pública deve ser implantada.  
  
    Para saber mais, confira: [Minimódulo do guia de laboratório de teste: PKI básico para Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   A rede corporativa deve ser o IPv6 habilitado. Se você estiver usando ISATAP, remova-o e use o IPv6 nativo.  
  
## <a name="in-this-scenario"></a>Neste cenário  
O cenário de implantação multissite inclui uma série de etapas:  
  
1. [Implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Um único servidor de acesso remoto com configurações avançadas deve ser implantado antes de configurar uma implantação multissite.  
  
2. [Planejar uma implantação multissite](plan/Plan-a-Multisite-Deployment.md). Para criar uma série de informações adicionais sobre planejamento de uma implantação multissite de um único servidor etapas são necessárias, incluindo conformidade com multissite pré-requisitos e planejamento de grupos de segurança do Active Directory, objetos de diretiva de grupo (GPOs), DNS e as configurações do cliente.  
  
3. [Configurar uma implantação multissite](configure/Configure-a-Multisite-Deployment.md). Isso consiste em um número de etapas de configuração, incluindo a preparação da infraestrutura do Active Directory, configuração do servidor de acesso remoto existente e a adição de vários servidores de acesso remoto como pontos de entrada à implantação multissite.  
  
4. [Solucionar problemas de uma implantação multissite](troubleshoot/Troubleshoot-a-Multisite-Deployment.md). Esta seção solução de problemas descreve vários dos erros mais comuns que podem ocorrer ao implantar o acesso remoto em uma implantação multissite.
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
Uma implantação multissite fornece o seguinte:  
  
-   Implantação multissite de um desempenho aprimorada permite que computadores cliente acessam recursos internos usando o acesso remoto para se conectar usando o ponto de entrada mais próximo e mais adequado. Cliente acessar com eficiência os recursos internos e a velocidade do cliente da que Internet solicita roteado por meio do DirectAccess é aprimorada. O tráfego nos pontos de entrada pode ser balanceado usando um balanceador de carga global externo.  
  
-   Facilidade de gerenciamento multissite permite que os administradores alinhem a implantação do acesso remoto a uma implantação de sites do Active Directory, fornecendo uma arquitetura simplificada. As configurações compartilhadas podem ser facilmente definidas em servidores do ponto de entrada ou clusters. Configurações de acesso remoto podem ser gerenciadas do qualquer um dos servidores na implantação, ou remotamente usando ferramentas de administração de servidor remoto (RSAT). Além disso, toda a implantação multissite pode ser monitorada em um único console de gerenciamento de acesso remoto.  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista as funções e recursos usados neste cenário.  
  
|Função/recurso|Como este cenário tem suporte|  
|---------|-----------------|  
|Função Acesso Remoto|A função é instalada e desinstalada pelo console Gerenciador do Servidor. Essa função engloba o DirectAccess, que era anteriormente um recurso no Windows Server 2008 R2 e Serviços de Roteamento e Acesso Remoto (RRAS) que eram anteriormente um serviço de função sob a função de servidor de Serviços de Acesso e Política de Rede (NPAS). A função Acesso Remoto consiste em dois componentes:<br /><br />-O DirectAccess e o roteamento e os serviços de acesso remoto (RRAS) VPN do DirectAccess e VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />-Recursos de roteamento de RRAS Roteamento-RRAS são gerenciados no console de roteamento e acesso remoto legado.<br /><br />As dependências são as seguintes:<br /><br />-O Internet Information Services (IIS) servidor Web - esse recurso é necessário para configurar o servidor de local de rede e a investigação da web padrão.<br />-Windows Database-Used interno para contabilidade local no servidor de acesso remoto.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<br /><br />– Ele é instalado por padrão em um servidor de acesso remoto quando a função acesso remoto está instalada e oferece suporte a interface de usuário do console de gerenciamento remoto.<br />-Ele pode ser instalado opcionalmente em um servidor que não executa a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<br /><br />O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<br /><br />-Ferramentas de linha de comando e GUI de acesso remoto<br />-Módulo de acesso remoto para o Windows PowerShell<br /><br />As dependências incluem:<br /><br />-Console de gerenciamento de diretiva de grupo<br />-Kit de administração do Gerenciador de Conexão RAS (CMAK)<br />-   Windows PowerShell 3.0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Os requisitos de hardware para este cenário incluem o seguinte:  
  
-   Pelo menos dois computadores de acesso remoto a serem coletadas em uma implantação multissite.   
  
-   Para testar o cenário, é necessário pelo menos um computador executando o Windows 8 e configurado como um cliente DirectAccess. Para testar o cenário para clientes que executam o Windows 7 é necessário pelo menos um computador executando o Windows 7.  
  
-   Para balancear carga de tráfego entre os servidores de ponto de entrada, é necessário um balanceador de carga global externo de terceiros.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Os requisitos de software para este cenário incluem:  
  
-   Requisitos de software para implantação de servidor único.  
  
-   Além dos requisitos de software para um único servidor, há uma série de requisitos multisite-específicos:  
  
    -   Autenticação IPsec em requisitos de que uma implantação multissite do DirectAccess deve ser implantada usando a autenticação de certificado de computador IPsec. Não há suporte para a opção de realizar a autenticação IPsec usando o servidor de acesso remoto como um proxy Kerberos. Uma CA interna é necessária para implantar os certificados de IPsec.  
  
    -   IP-HTTPS e a rede local requisitos-certificados de servidor necessárias para IP-HTTPS e o servidor de local de rede devem ser emitidos por uma autoridade de certificação. Não há suporte para a opção de usar certificados emitidos e auto-assinados pelo servidor de acesso remoto automaticamente. Certificados podem ser emitidos por uma AC interna ou por uma autoridade de certificação externa de terceiros.  
  
    -   Requisitos do Active Directory-pelo menos um site do Active Directory é necessário. O servidor de acesso remoto deve estar localizado no site. Para tempos de atualização, é recomendável que cada site tem um controlador de domínio gravável, embora isso não é obrigatório.  
  
    -   Requisitos de requisitos de grupo de segurança são da seguinte maneira:  
  
        -   Um único grupo de segurança é necessário para todos os computadores de cliente do Windows 8 de todos os domínios. É recomendável criar um grupo de segurança exclusivo desses clientes para cada domínio.  
  
        -   Um grupo de segurança exclusivo que contém computadores com Windows 7 é necessário para cada ponto de entrada configurado para dar suporte a clientes do Windows 7. É recomendável ter um grupo de segurança exclusivo para cada ponto de entrada em cada domínio.  
  
        -   Computadores não devem ser incluídos em mais de um grupo de segurança que inclua clientes do DirectAccess. Se os clientes forem incluídos em diversos grupos, a resolução do nome dos grupos para solicitações de clientes não funcionará conforme esperado.  
  
    -   Requisitos de GPO-GPOs podem ser criadas manualmente antes de configurar o acesso remoto, ou criados automaticamente durante a implantação do acesso remoto. Requisitos são da seguinte maneira:  
  
        -   Um GPO de cliente exclusivos é necessário para cada domínio.  
  
        -   Um GPO de servidor é necessária para cada ponto de entrada no domínio em que o ponto de entrada está localizado. Portanto, se vários pontos de entrada estão localizados no mesmo domínio, haverá vários GPOs (uma para cada ponto de entrada) no domínio do servidor.  
  
        -   Um GPO de cliente exclusivo do Windows 7 é necessário para cada ponto de entrada habilitado para suporte de cliente do Windows 7, para cada domínio.  
  
## <a name="KnownIssues"></a>Problemas conhecidos  
Os seguintes problemas conhecidos quando se configura um cenário multissite:  
  
-   **Vários pontos de entrada na mesma sub-rede IPv4**. Adicionar vários pontos de entrada na mesma sub-rede IPv4 resultará em uma mensagem de conflito de endereço IP e o endereço do DNS64 para o ponto de entrada não será configurado como esperado. Esse problema ocorre quando o IPv6 não foi implantada nas interfaces dos servidores na rede corporativa internas. Para evitar esse problema, execute o seguinte comando do Windows PowerShell em todos os servidores de acesso remoto atuais e futuros:  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   Se o endereço público especificado para os clientes DirectAccess para se conectar ao servidor de acesso remoto tem um sufixo incluído na NRPT, o DirectAccess podem não funcionar conforme o esperado. Certifique-se de que a NRPT tem uma isenção para o nome público. Em uma implantação multissite, isenções devem ser adicionadas para os nomes públicos de todos os pontos de entrada. Observe que, se túnel à força estiver habilitada essas isenções serão adicionadas automaticamente. Eles serão removidos se criar túneis à força estiver desabilitado.  
  
-   Ao usar o cmdlet do Windows PowerShell **Disable-DAMultiSite**, os parâmetros WhatIf e Confirm tem nenhum efeito e o multissite será desabilitado e GPOs do Windows 7 será removido.  
  
-   Quando os clientes do Windows 7 usando o DCA em uma implantação multissite são atualizados para o Windows 8, o Assistente de conectividade de rede não funcionará. Esse problema pode ser resolvido com antecedência sobre a atualização do cliente, modificando os GPOs de 7 do Windows usando os seguintes cmdlets do Windows PowerShell:  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    No caso em que o cliente já tiver sido atualizado, mova o computador cliente para o grupo de segurança do Windows 8.  
  
-   Ao modificar as configurações de controlador de domínio usando o cmdlet do Windows PowerShell **Set-DAEntryPointDC**, se o parâmetro ComputerName especificado é um servidor de acesso remoto em um ponto de entrada diferente do último adicionado para o multissite implantação, um aviso será exibida indicando que o servidor especificado não será atualizado até a próxima atualização de política. Os servidores reais que não foi atualizados podem ser vistos usando o **Status de configuração** na **DASHBOARD** da **Console de gerenciamento de acesso remoto**. Isso não causará problemas funcionais, no entanto, você pode executar **gpupdate /force** nos servidores que não foi atualizados para obter o status de configuração atualizado imediatamente.  
  
-   Quando o multissite é implantado em uma somente com IPv4 rede corporativa, alterando o interno endereço de rede prefixo IPv6 também alterações DNS64, mas não atualiza o endereço em regras de firewall que permitem que consultas DNS para o serviço DNS64. Para resolver esse problema, execute os seguintes comandos do Windows PowerShell depois de alterar o prefixo IPv6 da rede interna:  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   Se o DirectAccess tenha sido implantado quando uma infraestrutura ISATAP existente estava presente durante a remoção de um ponto de entrada foi um host ISATAP, o endereço IPv6 do DNS64 serviço será removido dos endereços de servidor DNS de todos os sufixos DNS na NRPT.  
  
    Para resolver esse problema, nos **instalação do servidor de infraestrutura** assistente no **DNS** página, remova os sufixos DNS que foram modificados e adicioná-los novamente com os endereços de servidor DNS corretos, clicando em  **Detectar** sobre o **endereços do servidor DNS** caixa de diálogo.  
  


