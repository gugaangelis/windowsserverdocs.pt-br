---
title: Plano da etapa 2 avançadas as implantações do DirectAccess
description: Este tópico faz parte do guia de implantar um único servidor DirectAccess com avançadas configurações para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3bba28d4-23e2-449f-8319-7d2190f68d56
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c946d5bbdf6e8660aaa9e47ced44aed91cfb71da
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283461"
---
# <a name="step-2-plan-advanced-directaccess-deployments"></a>Plano da etapa 2 avançadas as implantações do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois de planejar a infraestrutura do DirectAccess, a próxima etapa para implantar o DirectAccess avançado em um único servidor com IPv4 e IPv6 é planejar as configurações do Assistente de configuração de acesso remoto.  
  
|Tarefa|Descrição|  
|----|--------|  
|[2.1 planejar a implantação do cliente](#21-plan-for-client-deployment)|Planeje como permitir que computadores cliente se conectem usando o DirectAccess. Decida quais computadores gerenciados serão configurados como clientes DirectAccess e planeje a implantação do Assistente de Conectividade de Rede ou do Assistente de Conectividade do DirectAccess em computadores cliente.|  
|[2.2 planejar a implantação do servidor DirectAccess](#22-plan-for-directaccess-server-deployment)|Planeje como implantar o servidor do DirectAccess.|  
|[2.3 planejar servidores de infraestrutura](#23-plan-infrastructure-servers)|Planeje os servidores de infraestrutura para a implantação do DirectAccess, incluindo o servidor local de rede do DirectAccess, os servidores de DNS e os servidores de gerenciamento do DirectAccess.|  
|[2.4 planejar os servidores de aplicativos](#24-plan-application-servers)|Planeje servidores de aplicativos IPv4 e IPv6 e, como opção, considere se a autenticação fim a fim entre computadores cliente DirectAccess e servidores de aplicativos internos é necessária.|  
|[2.5 planejar DirectAccess e clientes VPN de terceiros](#25-plan-directaccess-and-third-party-vpn-clients)|Quando o DirectAccess é implantado com clientes VPN de terceiros, pode ser necessário definir os seguintes valores do Registro para habilitar a coexistência perfeita das duas soluções de acesso remoto.|  
  
## <a name="21-plan-for-client-deployment"></a>2.1 Planejar a implantação do cliente  
Existem três decisões que precisam ser tomadas ao planejar a implantação do cliente:  
  
1.  O DirectAccess estará disponível somente para computadores móveis, ou para qualquer computador?  
  
    Ao configurar os clientes do DirectAccess no Assistente de configuração de cliente do DirectAccess, você pode escolher permitir que somente computadores móveis nos grupos de segurança especificados se conectem usando o DirectAccess. Se você restringir o acesso a computadores móveis, o Acesso Remoto configurará automaticamente um filtro WMI para garantir que o GPO do cliente do DirectAccess seja aplicado apenas a computadores móveis nos grupos de segurança especificados. O administrador do Acesso Remoto requer permissões de segurança de Criação e Modificação para criar ou modificar filtros de WMI de GPOs para habilitar esta configuração.  
  
2.  Quais grupos de segurança conterão os computadores cliente DirectAccess?  
  
    As configurações de cliente do DirectAccess estão contidas no GPO do cliente do DirectAccess. O GPO é aplicado a computadores que fazem parte dos grupos de segurança que você especificou no Assistente de configuração do cliente do DirectAccess. Você pode especificar que os grupos de segurança podem vir de qualquer domínio com suport. Para obter mais informações, consulte a seção [1.7 planejar Active Directory Domain Services](da-adv-plan-s1-infrastructure.md#17-plan-active-directory-domain-services).  
  
    Antes de configurar o DirectAccess, você deverá criar os grupos de segurança. Você pode adicionar computadores aos grupos de segurança após concluir a implantação do DirectAccess, mas se adicionar computadores cliente que residem em um domínio diferente do grupo de segurança, o GPO do cliente não será aplicado a tais clientes. Por exemplo, se você criou o SG1 no domínio A para clientes do DirectAccess e posteriormente adicionar clientes do domínio B a este grupo, o GPO do cliente não será aplicado aos clientes do domínio B. Para evitar este problema, crie um novo grupo de segurança para cada domínio que contiver computadores cliente do DirectAccess. Como alternativa, se você não desejar criar um novo grupo de segurança, execute o cmdlet do Windows PowerShell **Add-DAClient** com o nome do novo GPO para o novo domínio.  
  
3.  Quais configurações serão definidas para o assistente de conectividade de rede?  
  
    O assistente de conectividade de rede é executado nos computadores cliente e fornece informações adicionais sobre a conexão do DirectAccess com os usuários finais. No Assistente de configuração do cliente do DirectAccess, você pode configurar o seguinte:  
  
    -   **Verificadores de conectividade**  
  
        Uma sonda de web padrão é criada para os clientes validarem a conectividade com a rede interna. O nome padrão é:  
  
        https://directaccess-WebProbeHost.<domain_name>  
  
        O nome deverá ser registrado manualmente no DNS. Você pode criar outros verificadores de conectividade usando outros endereços da web em HTTP ou **ping**. Uma entrada de DNS deverá existir para cada verificador de conectividade.  
  
    -   **Um endereço de email de assistência técnica**  
  
        Se os usuários finais enfrentarem problemas de conectividade com o DirectAccess, eles poderão enviar um email contendo informações de diagnóstico para que o administrador do DirectAccess possa solucionar o problema.  
  
    -   **Um nome de conexão do DirectAccess**  
  
        Especifique um nome de conexão para ajudar os usuários finais a identificarem a conexão do DirectAccess no computador.  
  
    -   **Permitir que os clientes do DirectAccess usar a resolução de nome local**  
  
        Os clientes requerem uma maneira de resolver nomes localmente. Se você permitir que os clientes do DirectAccess usem a resolução de nome local, os usuários finais poderão usar os servidores de DNS local para resolver os nomes. Quando os usuários finais escolherem usar os servidores de DNS para resoluções de nome, o DirectAccess não enviará solicitações de resolução para nomes de rótulo único para o servidor de DNS corporativo interno. Ele usará a resolução de nome local (usando o LLMNR (Link-Local Multicast Name Resolution) e o NetBIOS por meio dos protocolos TCP/IP).  
  
## <a name="22-plan-for-directaccess-server-deployment"></a>2.2 Planejar a implantação do servidor do DirectAccess  
Considere as seguintes decisões ao planejar a implantação do servidor do DirectAccess:  
  
-   **Topologia de rede**  
  
    Há algumas topologias disponíveis ao implantar um servidor do DirectAccess:  
  
    -   **Dois adaptadores de rede**. Com dois adaptadores de rede, o DirectAccess pode ser configurado com um adaptador de rede conectado diretamente à Internet e o outro à rede interna. Ou, então, o servidor pode ser instalado atrás de um dispositivo de borda como um firewall ou um roteador. Nesta configuração, um adaptador de rede está conectado à rede de perímetro e o outro à rede interna.  
  
    -   **Um adaptador de rede**. Nesta configuração, o servidor do DirectAccess é instalado atrás de um dispositivo de borda como um firewall ou um roteador. O adaptador de rede é conectado à rede interna.  
  
    Para obter mais informações sobre como selecionar a topologia para sua implantação, consulte [1.1 topologia de rede do plano e as configurações de](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings).  
  
-   **Endereço ConnectTo**  
  
    Os computadores cliente usam o endereço ConnectTo para conectar-se ao servidor do DirectAccess. O endereço escolhido deve ser correspondente ao nome do assunto do certificado IP-HTTPS implantado para a conexão IP-HTTPS, devendo também estar disponível no DNS público.  
  
-   **Adaptadores de rede**  
  
    O Assistente de configuração de servidor de acesso remoto detecta automaticamente os adaptadores de rede configurados no servidor do DirectAccess. Verifique se os adaptadores corretos foram selecionados.  
  
-   **Certificado IP-HTTPS**  
  
    O Assistente de configuração de servidor de acesso remoto detecta automaticamente um certificado adequado para a conexão IP-HTTPS. O nome do assunto do certificado selecionado deve ser correspondente ao endereço ConnectTo. Se você usar certificados autoassinados, poderá selecionar um certificado criado automaticamente pelo servidor do Acesso Remoto.  
  
-   **Prefixos IPv6**  
  
    Se o Assistente de configuração do servidor de acesso remoto detectar que o IPv6 foi implantado nos adaptadores de rede, ele preencherá automaticamente os prefixos de IPv6 para a rede interna, outro para a atribuição dos computadores cliente do DirectAccess e outro para a atribuição aos computadores cliente do VPN. Se os prefixos gerados automaticamente não estiverem corretos para sua infraestrutura IPv6 nativa, será necessário alterá-los manualmente. Para obter mais informações, consulte [1.1 topologia de rede do plano e as configurações de](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings).  
  
-   **Autenticação**  
  
    Decida como os clientes do DirectAccess serão autenticados no servidor do DirectAccess:  
  
    -   **Autenticação do usuário**. Você pode habilitar os usuários para autenticação com credenciais do Active Directory ou com autenticação em dois fatores. Para obter mais informações sobre a autenticação com a autenticação de dois fatores, consulte [implantar o acesso remoto com autenticação OTP](https://technet.microsoft.com/library/hh831379.aspx).  
  
    -   **Autenticação do computador**. Você pode configurar autenticação do computador para usar certificados ou o servidor do DirectAccess como proxy Kerberos em nome do cliente. Para obter mais informações, consulte [1.3 requisitos de certificado do plano](da-adv-plan-s1-infrastructure.md#13-plan-certificate-requirements).  
  
    -   **Os clientes do Windows 7**. Por padrão, os computadores cliente que executam o Windows 7 não podem se conectar a uma implantação do Windows Server 2012 R2 ou o DirectAccess do Windows Server 2012. Se você tiver clientes em sua organização que executam o Windows 7, e eles requerem acesso remoto aos recursos internos, você pode permitir que eles se conectem. Qualquer computador cliente que poderá receber acesso aos recursos internos deverá ser membro de um grupo de segurança especificado no Assistente de configuração do cliente do DirectAccess.  
  
        > [!NOTE]  
        > Permitindo que os clientes que executam o Windows 7 para se conectar usando o DirectAccess requer usar a autenticação de certificado do computador.  
  
-   **Configuração de VPN**  
  
    Antes de configurar o DirectAccess, decida se você fornecerá acesso por VPN a clientes remotos não compatíveis com o DirectAccess. Você deverá fornecer acesso por VPN se possuir computadores cliente na organização que não dão suporte à conectividade do DirectAccess (pois não são gerenciados ou por não executarem um sistema operacional que dá suporte a DirectAccess). O Assistente de configuração de servidor de acesso remoto permite configurar como os endereços IP são atribuídos (usando DHCP ou em um pool de endereços estáticos) e como os clientes VPN são autenticados, usando o Active Directory ou um servidor RADIUS (Remote Authentication Dial-Up Service).  
  
## <a name="23-plan-infrastructure-servers"></a>2.3 Planejar os servidores de infraestrutura  
O DirectAccess requer três tipos de servidores de infraestrutura:  
  
-   **Servidores DNS**. Para mais informações, consulte [1.4 Planejar os requisitos de DNS](da-adv-plan-s1-infrastructure.md#14-plan-dns-requirements)  
  
-   **Servidor de local de rede**. Para mais informações, consulte [1.5 Planejar o servidor de local de rede](da-adv-plan-s1-infrastructure.md#15-plan-the-network-location-server)  
  
-   **Servidores de gerenciamento**. Para mais informações, consulte [1.6 Planejar os servidores de gerenciamento](da-adv-plan-s1-infrastructure.md#16-plan-management-servers)  
  
## <a name="24-plan-application-servers"></a>2.4 Planejar os servidores de aplicativo  
Servidores de aplicativo são servidores na rede corporativa acessíveis pelos computadores cliente por meio de uma conexão do DirectAccess. Os servidores de aplicativo são identificados ao adicioná-los a um grupo de segurança. O GPO do servidor de aplicativos é, então, aplicado aos servidores neste grupo.  
  
> [!NOTE]  
> É necessário adicionar servidores de aplicativo a um grupo de segurança somente se a autenticação fim a fim ou a criptografia for requerida.  
  
Como opção, você pode requerer autenticação fim a fim e criptografia entre o cliente do DirectAccess e os servidores de aplicativo internos selecionados. Se você configurar a autenticação fim a fim, os clientes do DirectAccess usarão uma política de transporte IPsec. Esta política requer que a autenticação e a proteção de tráfego de sessões IPsec sejam encerradas nos servidores de aplicativo especificados. Neste caso, o servidor de Acesso Remoto encaminha as sessões de IPsec autenticadas e protegidas para os servidores de aplicativos.  
  
Por padrão, quando você estende a autenticação a servidores de aplicativos, a carga de dados é criptografada entre o cliente do DirectAccess e o servidor de aplicativos. Você pode escolher não criptografar o tráfego e usar somente autenticação. No entanto, isso é menos seguro do que usar a autenticação e criptografia, e ele tem suporte apenas para servidores de aplicativos que executam o Windows Server 2008 R2 ou sistemas operacionais Windows Server 2012.  
  
## <a name="25-plan-directaccess-and-third-party-vpn-clients"></a>2.5 Planejar DirectAccess e clientes VPN de terceiros  
Alguns clientes VPN de terceiros não criam conexões na pasta Conexões de Rede. Isso pode levar o DirectAccess a determinar que não há conectividade de intranet quando a conexão VPN é estabelecida, embora haja conectividade com a Intranet. Isso ocorre quando clientes VPN de terceiros registram as interfaces definindo-as como tipos de ponto de extremidade NDIS (especificação da interface do dispositivo de rede). Você pode habilitar a coexistência com esses tipos de clientes VPN definindo o valor do Registro como 1 nos clientes do DirectAccess:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\ShowDomainEndpointInterfaces (REG_DWORD)**  
  
Alguns clientes VPN de terceiros usam uma configuração de túnel dividido, que permite ao computador cliente VPN acessar a Internet diretamente, sem precisar enviar o tráfego pela conexão VPN para a Intranet.  
  
As configurações de túnel dividido, em geral, deixam a configuração do Gateway Padrão no cliente VPN como não configurada ou como somente zeros (0.0.0.0). Você pode confirmar isso estabelecendo com êxito uma conexão VPN com a sua Intranet e usando a ferramenta de linha de comando Ipconfig.exe para exibir a configuração resultante.  
  
Se a conexão do VPN listar o gateway padrão como vazio ou somente zeros (0.0.0.0), seu cliente de VPN está configurado desta maneira. Por padrão, o cliente DirectAccess não identifica configurações de túnel dividido. Para configurar clientes DirectAccess para detectarem esses tipos de configurações de cliente VPN e coexistirem com elas, defina o valor do Registro a seguir como 1:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\Internet\ EnableNoGatewayLocationDetection (REG_DWORD)**  
  
## <a name="previous-step"></a>Etapa anterior  
  
-   [Etapa 1: Planejar a infraestrutura do DirectAccess](da-adv-plan-s1-infrastructure.md)  
  


