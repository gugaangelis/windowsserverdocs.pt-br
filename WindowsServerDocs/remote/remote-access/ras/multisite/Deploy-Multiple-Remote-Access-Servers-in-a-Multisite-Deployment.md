---
title: Implantar vários servidores de acesso remoto em uma implantação multissite
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8f0d8b4416c8480921d43fd4e705b837082152fb
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991349"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>Implantar vários servidores de acesso remoto em uma implantação multissite

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

 O Windows Server 2016 e o Windows Server 2012 combinam o DirectAccess e o serviço de acesso remoto (RAS) VPN em uma única função de acesso remoto. O Acesso Remoto pode ser implantado em diversos cenários corporativos. Esta visão geral fornece uma introdução ao cenário empresarial para a implantação de servidores de acesso remoto em uma configuração multissite.

## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrição do cenário
Em uma implantação multissite, dois ou mais servidores de acesso remoto ou clusters de servidores são implantados e configurados como pontos de entrada diferentes em um único local ou em locais geográficos dispersos. A implantação de vários pontos de entrada em um único local permite a redundância do servidor ou o alinhamento dos servidores de acesso remoto com a arquitetura de rede existente. A implantação por localização geográfica garante o uso eficiente de recursos, pois os computadores cliente remotos podem se conectar a recursos de rede internos usando um ponto de entrada mais próximo a eles. O tráfego em uma implantação multissite pode ser distribuído e equilibrado com um balanceador de carga global externo.

Uma implantação multissite dá suporte A computadores cliente que executam o Windows 10, Windows 8 ou Windows 7. Os computadores cliente que executam o Windows 10 ou o Windows 8 identificam automaticamente um ponto de entrada ou o usuário pode selecionar manualmente um ponto de entrada. A atribuição automática ocorre na seguinte ordem de prioridade:

1.  Use um ponto de entrada selecionado manualmente pelo usuário.

2.  Use um ponto de entrada identificado por um balanceador de carga global externo se um for implantado.

3.  Use o ponto de entrada mais próximo identificado pelo computador cliente usando um mecanismo de investigação automático.

O suporte para clientes que executam o Windows 7 deve ser habilitado manualmente em cada ponto de entrada e não há suporte para a seleção de um ponto de entrada por esses clientes.

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar a implantar este cenário, examine esta lista de requisitos importantes:

-   [Implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) deve ser implantado antes de uma implantação multissite.

-   Os clientes do Windows 7 sempre se conectarão a um site específico. Eles não poderão se conectar ao site mais próximo com base no local do cliente (ao contrário dos clientes do Windows 10, 8 ou 8,1).

-   Não há suporte para alteração de políticas fora do console de gerenciamento do DirectAccess ou dos cmdlets do PowerShell.

-   Uma infraestrutura de chave pública deve ser implantada.

    Para saber mais, veja os tópicos sobre: [Minimódulo de guia do laboratório de teste: PKI Básico para Windows Server 2012.](/answers/topics/windows-server-2012.html)

-   A rede corporativa deve estar habilitada para IPv6. Se você estiver usando ISATAP, remova-o e use o IPv6 nativo.

## <a name="in-this-scenario"></a>Neste cenário
O cenário de implantação multissite inclui várias etapas:

1. [Implante um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Um único servidor de acesso remoto com configurações avançadas deve ser implantado antes de configurar uma implantação multissite.

2. [Planejar uma implantação multissite](plan/Plan-a-Multisite-Deployment.md). Para criar uma implantação multissite a partir de um único servidor, várias etapas de planejamento adicionais são necessárias, incluindo a conformidade com os pré-requisitos multissite e o planejamento de Active Directory grupos de segurança, objetos de Política de Grupo (GPOs), DNS e configurações do cliente.

3. [Configurar uma implantação multissite](configure/Configure-a-Multisite-Deployment.md). Isso consiste em várias etapas de configuração, incluindo a preparação da infraestrutura de Active Directory, a configuração do servidor de acesso remoto existente e a adição de vários servidores de acesso remoto como pontos de entrada para a implantação multissite.

4. [Solucionar problemas de implantação multissite](troubleshoot/Troubleshoot-a-Multisite-Deployment.md). Esta seção de solução de problemas descreve uma série de erros mais comuns que podem ocorrer ao implantar o acesso remoto em uma implantação multissite.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicações práticas
Uma implantação multissite fornece o seguinte:

-   Desempenho aprimorado-uma implantação multissite permite que computadores cliente acessem recursos internos usando o acesso remoto para se conectar usando o ponto de entrada mais próximo e adequado. O cliente acessa recursos internos com eficiência e a velocidade das solicitações de Internet do cliente roteadas por meio do DirectAccess é aprimorada. O tráfego entre pontos de entrada pode ser balanceado usando um balanceador de carga global externo.

-   A facilidade de gerenciamento – multissite permite que os administradores alinhem a implantação de acesso remoto a uma implantação de Active Directory sites, fornecendo uma arquitetura simplificada. As configurações compartilhadas podem ser facilmente definidas em clusters ou servidores de ponto de entrada. As configurações de acesso remoto podem ser gerenciadas de qualquer um dos servidores na implantação ou remotamente usando Ferramentas de Administração de Servidor Remoto (RSAT). Além disso, toda a implantação multissite pode ser monitorada em um único console de gerenciamento de acesso remoto.

## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário
A tabela a seguir lista as funções e os recursos usados neste cenário.

|Função/recurso|Como este cenário tem suporte|
|---------|-----------------|
|Função Acesso Remoto|A função é instalada e desinstalada pelo console Gerenciador do Servidor. Essa função engloba o DirectAccess, que era anteriormente um recurso no Windows Server 2008 R2 e Serviços de Roteamento e Acesso Remoto (RRAS) que eram anteriormente um serviço de função sob a função de servidor de Serviços de Acesso e Política de Rede (NPAS). A função Acesso Remoto consiste em dois componentes:<p>-DirectAccess e roteamento e serviços de acesso remoto (RRAS) VPN-e VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />-Os recursos de roteamento RRAS de roteamento RRAS são gerenciados no console de roteamento e acesso remoto herdado.<p>As dependências são as seguintes:<p>-Serviços de Informações da Internet (IIS) servidor Web-esse recurso é necessário para configurar o servidor de local de rede e a investigação da Web padrão.<br />-Banco de dados interno do Windows-usado para contabilização local no servidor de acesso remoto.|
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<p>-Ele é instalado por padrão em um servidor de acesso remoto quando a função de acesso remoto é instalada e dá suporte à interface do usuário do console de gerenciamento remoto.<br />-Ele pode ser instalado opcionalmente em um servidor que não está executando a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<p>O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<p>-GUI de acesso remoto e ferramentas de linha de comando<br />-Módulo de acesso remoto para Windows PowerShell<p>As dependências incluem:<p>-Console de Gerenciamento de Política de Grupo<br />-Kit de administração do Gerenciador de conexões RAS (CMAK)<br />-Windows PowerShell 3,0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|

## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisitos de hardware
Os requisitos de hardware para este cenário incluem o seguinte:

-   Pelo menos dois computadores de acesso remoto a serem coletados em uma implantação multissite.

-   Para testar o cenário, é necessário pelo menos um computador executando o Windows 8 e configurado como um cliente do DirectAccess. Para testar o cenário para clientes que executam o Windows 7, pelo menos um computador com o Windows 7 é necessário.

-   Para balancear a carga do tráfego entre servidores de ponto de entrada, é necessário um balanceador de carga global externo de terceiros.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
Os requisitos de software para este cenário incluem:

-   Requisitos de software para implantação de servidor único.

-   Além dos requisitos de software para um único servidor, há uma série de requisitos específicos para vários sites:

    -   Requisitos de autenticação IPsec-em um DirectAccess de implantação multissite devem ser implantados usando a autenticação de certificado de computador IPsec. Não há suporte para a opção de executar a autenticação IPsec usando o servidor de acesso remoto como um proxy Kerberos. Uma AC interna é necessária para implantar os certificados IPsec.

    -   Requisitos de servidor IP-HTTPS e de local de rede-certificados necessários para IP-HTTPS e o servidor de local de rede devem ser emitidos por uma autoridade de certificação. Não há suporte para a opção de usar certificados emitidos e assinados automaticamente pelo servidor de acesso remoto. Os certificados podem ser emitidos por uma CA interna ou por uma CA externa de terceiros.

    -   Requisitos de Active Directory-pelo menos um site de Active Directory é necessário. O servidor de acesso remoto deve estar localizado no site. Para tempos de atualização mais rápidos, é recomendável que cada site tenha um controlador de domínio gravável, embora isso não seja obrigatório.

    -   Requisitos do grupo de segurança – os requisitos são os seguintes:

        -   Um único grupo de segurança é necessário para todos os computadores cliente com Windows 8 de todos os domínios. É recomendável criar um grupo de segurança exclusivo desses clientes para cada domínio.

        -   Um grupo de segurança exclusivo contendo computadores com Windows 7 é necessário para cada ponto de entrada configurado para dar suporte a clientes do Windows 7. É recomendável ter um grupo de segurança exclusivo para cada ponto de entrada em cada domínio.

        -   Computadores não devem ser incluídos em mais de um grupo de segurança que inclua clientes do DirectAccess. Se os clientes forem incluídos em diversos grupos, a resolução do nome dos grupos para solicitações de clientes não funcionará conforme esperado.

    -   Requisitos de GPO-os GPOs podem ser criados manualmente antes de configurar o acesso remoto ou criados automaticamente durante a implantação de acesso remoto. Os requisitos são os seguintes:

        -   Um GPO de cliente exclusivo é necessário para cada domínio.

        -   Um GPO de servidor é necessário para cada ponto de entrada, no domínio no qual o ponto de entrada está localizado. Portanto, se vários pontos de entrada estiverem localizados no mesmo domínio, haverá vários GPOs de servidor (um para cada ponto de entrada) no domínio.

        -   Um GPO de cliente do Windows 7 exclusivo é necessário para cada ponto de entrada habilitado para o suporte ao cliente do Windows 7, para cada domínio.

## <a name="known-issues"></a><a name="KnownIssues"></a>Problemas conhecidos
Veja a seguir os problemas conhecidos ao configurar um cenário multissite:

-   **Vários pontos de entrada na mesma sub-rede IPv4**. A adição de vários pontos de entrada na mesma sub-rede IPv4 resultará em uma mensagem de conflito de endereço IP e o endereço DNS64 para o ponto de entrada não será configurado conforme o esperado. Esse problema ocorre quando o IPv6 não foi implantado nas interfaces internas dos servidores na rede corporativa. Para evitar esse problema, execute o seguinte comando do Windows PowerShell em todos os servidores de acesso remoto atuais e futuros:

    ```
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0
    ```

-   Se o endereço público especificado para clientes DirectAccess se conectar ao servidor de acesso remoto tiver um sufixo incluído na NRPT, o DirectAccess poderá não funcionar conforme o esperado. Verifique se a NRPT tem uma isenção para o nome público. Em uma implantação multissite, as isenções devem ser adicionadas para os nomes públicos de todos os pontos de entrada. Observe que, se o túnel forçado estiver habilitado, essas isenções serão adicionadas automaticamente. Eles serão removidos se o túnel forçado estiver desabilitado.

-   Ao usar o cmdlet **Disable-DAMultiSite**do Windows PowerShell, os parâmetros WhatIf e Confirm não têm efeito e o multissite será desabilitado e os GPOs do Windows 7 serão removidos.

-   Quando os clientes do Windows 7 que usam o DCA em uma implantação multissite forem atualizados para o Windows 8, o assistente de conectividade de rede não funcionará. Esse problema pode ser resolvido antes da atualização do cliente, modificando os GPOs do Windows 7 usando os seguintes cmdlets do Windows PowerShell:

    ```
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"
    ```

    Caso o cliente já tenha sido atualizado, mova o computador cliente para o grupo de segurança do Windows 8.

-   Ao modificar as configurações do controlador de domínio usando o cmdlet do Windows PowerShell **set-DAEntryPointDC**, se o parâmetro ComputerName especificado for um servidor de acesso remoto em um ponto de entrada diferente do último adicionado à implantação multissite, um aviso será exibido indicando que o servidor especificado não será atualizado até a próxima atualização da política. Os servidores reais que não foram atualizados podem ser vistos usando o **status de configuração** no **painel** do console de gerenciamento de **acesso remoto**. No entanto, isso não causará problemas funcionais, mas você pode executar **gpupdate/force** nos servidores que não foram atualizados para obter o status da configuração atualizado imediatamente.

-   Quando o multissite é implantado em uma rede corporativa somente IPv4, alterar o prefixo IPv6 da rede interna também altera o endereço DNS64, mas não atualiza o endereço em regras de firewall que permitem consultas DNS ao serviço DNS64. Para resolver esse problema, execute os seguintes comandos do Windows PowerShell depois de alterar o prefixo IPv6 da rede interna:

    ```
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers

    $serverGpoName = (Get-RemoteAccess).ServerGpoName

    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName

    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc

    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address

    Save-NetGPO -GPOSession $gpoSession
    ```

-   Se o DirectAccess foi implantado quando uma infraestrutura ISATAP existente estava presente, ao remover um ponto de entrada que era um host ISATAP, o endereço IPv6 do serviço DNS64 será removido dos endereços de servidor DNS de todos os sufixos DNS na NRPT.

    Para resolver esse problema, no assistente de **instalação do servidor de infraestrutura** , na página **DNS** , remova os sufixos DNS que foram modificados e adicione-os novamente com os endereços corretos do servidor DNS, clicando em **detectar** na caixa de diálogo **endereços do servidor DNS** .