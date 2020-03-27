---
title: Adicionar o DirectAccess a uma implantação de acesso remoto existente (VPN)
description: Este tópico faz parte do guia adicionar o DirectAccess a uma implantação de VPN (acesso remoto) existente para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5db01f7-1ae0-46f2-9be7-8d9e121446b2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1ad1b823cf48a2c322c7ccab1799c76993b1e9bf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314810"
---
# <a name="add-directaccess-to-an-existing-remote-access-vpn-deployment"></a>Adicionar o DirectAccess a uma implantação de acesso remoto existente (VPN)

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrição do cenário  
Nesse cenário, um único computador executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 é configurado como um servidor DirectAccess com as configurações recomendadas depois que você já tiver instalado e configurado a VPN. Se você quiser configurar o DirectAccess com recursos corporativos, como um cluster com balanceamento de carga, implantação multissite ou autenticação de cliente de dois fatores, conclua o cenário descrito neste tópico para configurar um único servidor e, em seguida, configure o cenário empresarial, conforme descrito em [implantar o acesso remoto em uma empresa](../../ras/Deploy-Remote-Access-in-an-Enterprise.md).  
  
## <a name="in-this-scenario"></a>Neste cenário  
Para configurar um único servidor de Acesso Remoto, várias etapas de planejamento e implantação são necessárias.  
  
### <a name="planning-steps"></a>Etapas de planejamento  
O planejamento está dividido em duas fases:  
  
1.  **Planejar a infraestrutura de acesso remoto**  
  
    Nesta fase, você descreve o planejamento necessário para configurar a infraestrutura de rede antes de começar a implantação do Acesso Remoto. Isso inclui planejar a rede e a topologia do servidor, certificados, DNS (Sistema de Nomes de Domínio), configuração do Active Directory e do GPO (Objeto de Política de Grupo) e o servidor de local de rede do DirectAccess.  
  
2.  **Planejar a implantação de acesso remoto**  
  
    Nesta fase, você descreve as etapas de planejamento necessárias para preparar a implantação do Acesso Remoto. Ela inclui o planejamento para computadores cliente de Acesso Remoto, requisitos de autenticação de servidor e cliente e servidores de infraestrutura.  
  
### <a name="deployment-steps"></a>Etapas de implantação  
A implantação está dividida em três fases:  
  
1.  **Configurar a infraestrutura de acesso remoto**  
  
    Nesta fase, você configura rede e roteamento, configurações de firewall (se necessário), certificados, servidores DNS, configurações do Active Directory e do GPO e o servidor de local de rede do DirectAccess.  
  
2.  **Definir configurações do servidor de acesso remoto**  
  
    Nesta fase, você configura os computadores cliente de Acesso Remoto, o servidor de Acesso Remoto e os servidores de infraestrutura.  
  
3.  **Verificar a implantação**  
  
    Nesta fase, verifique se a implantação está funcionando conforme o necessário.  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicativos práticos  
A implantação de um só servidor de Acesso Remoto oferece:  
  
-   **Facilidade de acesso**  
  
    Os computadores cliente gerenciados que executam o Windows 8 e o Windows 7 podem ser configurados como computadores cliente do DirectAccess. Esses clientes podem acessar os recursos da rede interna por meio do DirectAccess sempre que estiverem localizados na Internet, sem precisar entrar em uma conexão VPN. Computadores cliente que não executam um desses sistemas operacionais podem se conectar à rede interna por meio de uma VPN. O DirectAccess e a VPN são gerenciados no mesmo console e com o mesmo conjunto de assistentes.  
  
-   **Facilidade de gerenciamento**  
  
    Computadores cliente DirectAccess com acesso à Internet podem ser gerenciados remotamente por administradores de acesso remoto usando o DirectAccess, mesmo quando os computadores cliente não estiverem localizados na rede corporativa interna. Os computadores cliente que não atendem aos requisitos corporativos podem ser corrigidos automaticamente por servidores de gerenciamento.  
  
## <a name="roles-and-features-required-for-this-scenario"></a><a name="BKMK_NEW"></a>Funções e recursos necessários para este cenário  
A tabela a seguir lista funções e recursos necessários para este cenário:  
  
|Função/recurso|Como este cenário tem suporte|  
|---------|-----------------|  
|Função Acesso Remoto|A função é instalada e desinstalada usando o console de Gerenciador do Servidor ou o Windows PowerShell. Essa função engloba o DirectAccess, que era anteriormente um recurso no Windows Server 2008 R2 e os Serviços de Roteamento e Acesso Remoto que eram anteriormente um serviço de função na função de servidor NPAS (Serviços de Acesso e Política de Rede). A função Acesso Remoto consiste em dois componentes:<br /><br />1. DirectAccess e roteamento e VPN RRAS (serviços de acesso remoto): gerenciados no console de gerenciamento de acesso remoto.<br />2. roteamento RRAS: gerenciado no console de roteamento e acesso remoto.<br /><br />A função Servidor de Acesso Remoto depende dos seguintes recursos de servidor:<br /><br />-Serviços de Informações da Internet (IIS) servidor Web: necessário para configurar o servidor de local de rede no servidor de acesso remoto e a investigação da Web padrão.<br />-Banco de dados interno do Windows: usado para contabilização local no servidor de acesso remoto.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<br /><br />-Por padrão, em um servidor de acesso remoto quando a função de acesso remoto está instalada. Dá suporte à interface de usuário do console de Gerenciamento Remoto e os cmdlets do Windows PowerShell.<br />-Opcionalmente, instalado em um servidor que não está executando a função de servidor de acesso remoto. Neste caso, ele é usado para o gerenciamento remoto de um computador de Acesso Remoto que executa DirectAccess e VPN.<br /><br />O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<br /><br />-GUI de acesso remoto<br />-Módulo de acesso remoto para Windows PowerShell<br /><br />As dependências incluem:<br /><br />-Console de Gerenciamento de Política de Grupo<br />-Kit de administração do Gerenciador de conexões RAS (CMAK)<br />-Windows PowerShell 3,0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|  
  
## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisitos de hardware  
Os requisitos de hardware para este cenário incluem o seguinte:  
  
**Requisitos do servidor**  
  
-   Um computador que atenda aos requisitos de hardware do Windows Server 2012.  
  
-   O servidor deve ter pelo menos um adaptador de rede instalado, habilitado e associado à rede interna. Quando são usados dois adaptadores, um deles deve estar conectado à rede corporativa interna e o outro, à rede externa (Internet).  
  
-   Se for necessário Teredo como um protocolo de transição de IPv4 para IPv6, o adaptador externo do servidor exigirá dois endereços IPv4 públicos consecutivos. O Assistente para Habilitar o DirectAccess não permite Teredo, mesmo que dois endereços IP consecutivos estejam presentes. Se um único endereço IP estiver disponível, apenas IP-HTTPS poderá ser usado como protocolo de transição.  
  
-   Pelo menos um controlador de domínio. O servidor de Acesso Remoto e os clientes do DirectAccess devem ser membros do domínio.  
  
-   O Assistente para Habilitar o DirectAccess requer certificados para IP-HTTPS e o servidor local de rede. Se a VPN SSTP já estiver usando um certificado, ele será reutilizado para IP-HTTPS. Se a VPN SSTP não estiver configurada, você poderá configurar um certificado para IP-HTTPS ou usar um certificado autoassinado criado automaticamente. Para o servidor de local de rede, você pode configurar um certificado ou usar um certificado autoassinado criado automaticamente.  
  
**Requisitos do cliente**  
  
-   Um computador cliente deve estar executando o Windows 8 ou o Windows 7.  
  
    > [!NOTE]  
    > Somente os seguintes sistemas operacionais podem ser usados como clientes DirectAccess: Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 Enterprise e Windows 7 Ultimate.  
  
**Requisitos do servidor de gerenciamento e infraestrutura**  
  
-   Durante o gerenciamento remoto de computadores cliente DirectAccess, os clientes iniciam a comunicação com servidores de gerenciamento, como controladores de domínio, servidores de configuração do System Center e servidores de HRA (Autoridade de Registro de Integridade) para serviços que incluem atualizações do Windows e antivírus e conformidade do cliente com a NAP (Proteção de Acesso à Rede). Os servidores necessários devem ser implantados antes de iniciar a implantação do Acesso Remoto.  
  
-   Se o acesso remoto exigir conformidade NAP do cliente, o NPS (Servidor de Políticas de Rede) e HRA deverão ser implantados antes do início da implantação do Acesso Remoto.  
  
-   É necessário um servidor DNS executando o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 com SP2.  
  
## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software  
Os requisitos de software para este cenário incluem:  
  
**Requisitos do servidor**  
  
-   O servidor de Acesso Remoto deve ser um membro do domínio. O servidor pode ser implantado na borda da rede interna, ou atrás de um firewall de borda ou outro dispositivo.  
  
-   Se o servidor de Acesso Remoto estiver localizado atrás de um firewall de borda ou dispositivo NAT (Conversão de Endereços de Rede), o dispositivo deverá ser configurado para permitir o tráfego de e para o servidor de Acesso Remoto.  
  
-   A pessoa que implanta o acesso remoto no servidor precisa de permissões do administrador local no servidor e permissões de usuário de domínio. Além disso, o administrador precisa de permissões para os GPOs utilizados na implantação do DirectAccess. Para aproveitar os recursos que restringem uma implantação do DirectAccess somente a computadores móveis, são necessárias permissões para criar um filtro WMI no controlador de domínio.  
  
**Requisitos do cliente de acesso remoto**  
  
-   Os clientes do DirectAccess devem ser membros do domínio. Os domínios que contêm clientes podem pertencer à mesma floresta do servidor de Acesso Remoto, ou ter uma relação de confiança bidirecional com a floresta ou o domínio do servidor de Acesso Remoto.  
  
-   Um grupo de segurança do Active Directory é necessário para conter os computadores que serão configurados como clientes do DirectAccess. Se um grupo de segurança não for especificado quando você definir as configurações de cliente do DirectAccess, por padrão, o GPO do cliente será aplicado a todos os computadores laptop (compatíveis com o DirectAccess) no grupo de segurança de Computadores de Domínio. Somente os seguintes sistemas operacionais podem ser usados como clientes DirectAccess: Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 Enterprise e Windows 7 Ultimate.  
  
    > [!NOTE]  
    > Recomendamos que você crie um grupo de segurança para cada domínio que contém os computadores que serão configurados como clientes do DirectAccess.  
  

  

