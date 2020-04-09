---
title: Implantar um único servidor de DirectAccess usando o Assistente de Introdução
description: Este tópico faz parte do guia implantar um único servidor DirectAccess usando o assistente de Introdução para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: eb0cf464-0668-40f8-8222-feb6bae6d3d5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 13b3fdea120a857cc0c8e890bba87c13823c3a38
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819559"
---
# <a name="deploy-a-single-directaccess-server-using-the-getting-started-wizard"></a>Implantar um único servidor de DirectAccess usando o Assistente de Introdução

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico fornece uma introdução ao cenário do DirectAccess que usa um único servidor DirectAccess e permite implantar o DirectAccess em algumas etapas fáceis.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Antes de iniciar a implantação, consulte a lista de configurações sem suporte, de problemas conhecidos e de pré-requisitos  
Você pode usar os tópicos a seguir para examinar os pré-requisitos e outras informações antes de implantar o DirectAccess.  
  
-   [Configurações do DirectAccess sem suporte](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [Pré-requisitos para implantação do DirectAccess](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrição do cenário  
Nesse cenário, um único computador executando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012, é configurado como um servidor DirectAccess com as configurações padrão em algumas etapas fáceis do assistente, sem a necessidade de definir as configurações de infraestrutura, como como uma autoridade de certificação (CA) ou grupos de segurança Active Directory.  
  
> [!NOTE]  
> Se você quiser configurar uma implantação avançada com configurações personalizadas, consulte [Deploy a Single DirectAccess Server with Advanced Settings](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
  
## <a name="in-this-scenario"></a>Neste cenário  
Para configurar um servidor DirectAccess básico, várias etapas de planejamento e implantação são necessárias.  
  
### <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}  
Antes de começar a implantar este cenário, examine esta lista de requisitos importantes:  
  
-   O Firewall do Windows deve estar habilitado em todos os perfis  
  
-   Esse cenário só tem suporte quando os computadores cliente estão executando o Windows 10, Windows 8.1 ou Windows 8.  
  
-   Não há suporte para ISATAP na rede corporativa. Se você estiver usando ISATAP, remova-o e use o IPv6 nativo.  
  
-   Uma infraestrutura de chave pública não é necessária.  
  
-   Não há suporte para implantação de autenticação de dois fatores. Credenciais de domínio são necessárias para autenticação.  
  
-   Implanta automaticamente o DirectAccess para todos os computadores móveis no domínio atual.  
  
-   Tráfego de Internet não passa pelo túnel do DirectAccess. Não há suporte para forçar configuração de túnel.  
  
-   O servidor DirectAccess é o servidor de local de rede.  
  
-   A NAP (Proteção de Acesso à Rede) não é permitida.  
  
-   Não há suporte para alteração de políticas fora do console de gerenciamento do DirectAccess ou dos cmdlets do PowerShell.  
  
-   Para implantar multissite, agora ou no futuro, primeiro [implante um único servidor DirectAccess com configurações avançadas](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
### <a name="planning-steps"></a>Etapas de planejamento  
O planejamento está dividido em duas fases:  
  
1.  Planejando a infraestrutura do DirectAccess. Esta fase descreve o planejamento necessário para configurar a infraestrutura de rede antes de começar a implantação do DirectAccess. Ela inclui o planejamento da topologia de redes e servidores, além do servidor de local de rede do DirectAccess.  
  
2.  Planejando a implantação do DirectAccess. Esta fase descreve as etapas de planejamento necessárias para preparar a implantação do DirectAccess. Ela inclui o planejamento para computadores cliente de DirectAccess, requisitos de autenticação de servidor e cliente, configurações de VPN, servidores de infraestrutura e servidores de gerenciamento e de aplicativos.  
  
Para obter as etapas de planejamento detalhadas, consulte [planejar uma implantação avançada do DirectAccess](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md).  
  
### <a name="deployment-steps"></a>Etapas de implantação  
A implantação está dividida em três fases:  
  
1.  Configurando a infraestrutura do DirectAccess – essa fase inclui a configuração de rede e roteamento, a definição de configurações de firewall, se necessário, a configuração de certificados, servidores DNS, configurações de Active Directory e GPO e o local de rede do DirectAccess servidor.  
  
2.  Definindo as configurações do servidor DirectAccess. Esta fase inclui etapas para configurar os computadores cliente de DirectAccess, o servidor do DirectAccess, os servidores de infraestrutura e os servidores de gerenciamento e de aplicativos.  
  
3.  Verificando a implantação. Essa fase inclui etapas para verificar se a implantação está funcionando conforme necessário.  
  
Para obter etapas detalhadas de implantação, consulte [Install and Configure Basic DirectAccess](../../../remote-access/directaccess/single-server-wizard/Install-and-Configure-Basic-DirectAccess.md).  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicativos práticos  
A implantação de um só servidor de Acesso Remoto oferece:  
  
-   Facilidade de acesso. Você pode configurar computadores cliente gerenciados que executam o Windows 10, Windows 8.1, Windows 8 ou Windows 7, como clientes DirectAccess. Esses clientes podem acessar os recursos da rede interna por meio do DirectAccess sempre que estiverem localizados na Internet sem precisar fazer logon em uma conexão VPN. Computadores cliente que não executem um desses sistemas operacionais podem se conectar à rede interna por meio de conexões VPN tradicionais.  
  
-   Facilidade de gerenciamento. Os computadores cliente do DirectAccess localizados na Internet podem ser gerenciados remotamente por administradores de Acesso Remoto pelo DirectAccess, mesmo quando não estão localizados na rede corporativa interna. Os computadores cliente que não atendem aos requisitos corporativos podem ser corrigidos automaticamente por servidores de gerenciamento. O DirectAccess e a VPN são gerenciados no mesmo console e com o mesmo conjunto de assistentes. Além disso, um ou mais servidores de Acesso Remoto podem ser gerenciados a partir de um único console de Gerenciamento de Acesso Remoto.  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista funções e recursos necessários para o cenário:  
  
|Função/recurso|Como este cenário tem suporte|  
|---------|-----------------|  
|Função Acesso Remoto|A função é instalada e desinstalada usando o console de Gerenciador do Servidor ou o Windows PowerShell. Essa função engloba o DirectAccess, que era anteriormente um recurso no Windows Server 2008 R2 e Serviços de Roteamento e Acesso Remoto que eram anteriormente um serviço de função sob a função de servidor de Serviços de Acesso e Política de Rede (NPAS). A função Acesso Remoto consiste em dois componentes:<p>1. DirectAccess e roteamento e VPN RRAS (serviços de acesso remoto). O DirectAccess e a VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />2. roteamento RRAS. Os recursos de roteamento do RRAS são gerenciados no console de roteamento e acesso remoto herdado.<p>A Função Servidor de Acesso Remoto depende dos seguintes recursos/funções de servidor:<p>-Serviços de Informações da Internet (IIS) servidor Web-esse recurso é necessário para configurar o servidor de local de rede no servidor de acesso remoto e a investigação da Web padrão.<br />-Banco de dados interno do Windows. Usado para contabilidade local no servidor de Acesso Remoto.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<p>-Ele é instalado por padrão em um servidor de acesso remoto quando a função de acesso remoto é instalada e dá suporte à interface do usuário do console de gerenciamento remoto e aos cmdlets do Windows PowerShell.<br />-Ele pode ser instalado opcionalmente em um servidor que não está executando a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<p>O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<p>-GUI de acesso remoto<br />-Módulo de acesso remoto para Windows PowerShell<p>As dependências incluem:<p>-Console de Gerenciamento de Política de Grupo<br />-Kit de administração do Gerenciador de conexões RAS (CMAK)<br />-Windows PowerShell 3,0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|  
  
## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisitos de hardware  
Os requisitos de hardware para este cenário incluem o seguinte:  
  
-   Requisitos de servidor:  
  
    -   Um computador que atenda aos requisitos de hardware do Windows Server 2016, do Windows Server 2012 R2 ou do Windows Server 2012.  
  
    -   O servidor deve ter pelo menos um adaptador de rede instalado, habilitado e conectado à rede interna. Quando são usados dois adaptadores, um deles deve estar conectado à rede corporativa interna e o outro, à rede externa (Internet ou rede privada).  
  
    -   Pelo menos um controlador de domínio. O servidor de Acesso Remoto e os clientes do DirectAccess devem ser membros do domínio.  
  
-   Requisitos do cliente:  
  
    -   Um computador cliente deve estar executando o Windows 10, Windows 8.1 ou Windows 8.  
  
        > [!IMPORTANT]  
        > Se alguns ou todos os seus computadores cliente estiverem executando o Windows 7, você deverá usar o assistente de instalação avançada. O assistente de instalação do Introdução descrito neste documento não oferece suporte a computadores cliente que executam o Windows 7. Consulte [implantar um único servidor DirectAccess com configurações avançadas](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) para obter instruções sobre como usar clientes do Windows 7 com o DirectAccess.  
  
        > [!NOTE]  
        > Somente os seguintes sistemas operacionais podem ser usados como clientes DirectAccess: Windows 10 Enterprise, Windows 8.1 Enterprise, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8 Enterprise, Windows Server 2008 R2, Windows 7 Enterprise e Windows 7 Ultimate.  
  
-   Requisitos do servidor de infraestrutura e gerenciamento:  
  
    -   Se a VPN estiver habilitada e um pool de endereços IP estáticos não estiver configurado, você deve implantar um servidor DHCP para alocar endereços IP automaticamente a clientes VPN.  
  
-   É necessário um servidor DNS que executa o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2008 SP2 ou o Windows Server 2008 R2.  
  
## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software  
Há diversos requisitos para este cenário:  
  
-   Requisitos de servidor:  
  
    -   O servidor de Acesso Remoto deve ser um membro do domínio. O servidor pode ser implantado na borda da rede interna, ou atrás de um firewall de borda ou outro dispositivo.  
  
    -   Se o servidor de Acesso Remoto estiver localizado atrás de um firewall de borda ou dispositivo de NAT, o dispositivo deve ser configurado para permitir o tráfego de e para o servidor de Acesso Remoto.  
  
    -   A pessoa que implanta o acesso remoto no servidor precisa de permissões de administrador local no servidor e permissões de usuário de domínio. Além disso, o administrador precisa de permissões para os GPOs utilizados na implantação do DirectAccess. Para aproveitar os recursos que restringem a implantação do DirectAccess somente a computadores móveis, são necessárias permissões para criar um filtro WMI no controlador de domínio.  
  
-   Requisitos de cliente de Acesso Remoto:  
  
    -   Os clientes do DirectAccess devem ser membros do domínio. Domínios que contêm clientes podem pertencer à mesma floresta que o servidor de acesso remoto ou tiver uma relação de confiança bidirecional com a floresta do servidor de acesso remoto.  
  
    -   Um grupo de segurança do Active Directory é necessário para conter os computadores que serão configurados como clientes do DirectAccess. Se um grupo de segurança não for especificado ao configurar as definições de cliente do DirectAccess, por padrão o GPO do cliente será aplicado em todos os computadores laptop no grupo de segurança de Computadores de Domínio. Somente os seguintes sistemas operacionais podem ser usados como clientes DirectAccess: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 Enterprise e Windows 7 Ultimate.  
  
## <a name="see-also"></a><a name="BKMK_LINKS"></a>Consulte também  
A tabela a seguir fornece links para recursos adicionais.  
  
|Tipo de conteúdo|Referências|  
|--------|-------|  
|**Acesso remoto no TechNet**|[TechCenter de acesso remoto](https://technet.microsoft.com/network/bb545655.aspx)|  
|**Ferramentas e configurações**|[Cmdlets do PowerShell de acesso remoto](https://technet.microsoft.com/library/hh918399.aspx)|  
|**Recursos da comunidade**|[Entradas do wiki do DirectAccess](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**Tecnologias relacionadas**|[Como funciona o IPv6](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


