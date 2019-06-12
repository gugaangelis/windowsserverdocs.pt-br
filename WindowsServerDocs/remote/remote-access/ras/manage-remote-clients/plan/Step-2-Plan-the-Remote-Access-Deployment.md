---
title: Etapa 2 planejar a implantação do acesso remoto
description: Este tópico faz parte do guia de clientes do DirectAccess de gerenciar remotamente no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7aec08a19759c98150cf7518643f634947c5133d
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805000"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>Etapa 2 planejar a implantação do acesso remoto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois de planejar para a infraestrutura que você pretende usar para configurar o servidor de acesso remoto único para o gerenciamento remoto de clientes DirectAccess, você está pronto para planejar as configurações que usará o Assistente de configuração de acesso remoto.  
  
> [!NOTE]  
> Antes de continuar com essas tarefas, consulte [etapa 1: Planejar a infraestrutura de acesso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Tarefa|Descrição|  
|----|--------|  
|[Planejar uma estratégia de implantação do cliente](#plan-a-client-deployment-strategy)|Decida quais computadores gerenciados serão configurados como clientes do DirectAccess.|  
|[Planejar uma estratégia de implantação do servidor de acesso remoto](#plan-a-remote-access-server-deployment-strategy)|Planeje como implantar o servidor de Acesso Remoto.|  
|[Planejar configurações de servidores de infra-estrutura](#plan-the-infrastructure-servers-configurations)|Planeje os servidores de infraestrutura em sua implantação do acesso remoto, incluindo o servidor de local de rede do DirectAccess, servidores DNS e servidores de gerenciamento do DirectAccess.|  
  
## <a name="plan-a-client-deployment-strategy"></a>Planejar uma estratégia de implantação do cliente  
Existem três decisões que precisam ser tomadas ao planejar a implantação do cliente:  
  
1.  O DirectAccess estará disponível para computadores móveis apenas, ou para todos os computadores em um grupo de segurança especificado?  
  
    Quando você configura os clientes do DirectAccess no Assistente de instalação do cliente do DirectAccess, você pode optar por permitir que somente computadores móveis nos grupos de segurança especificados para se conectar ao servidor usando o DirectAccess. Se você restringir o acesso a computadores móveis, o Acesso Remoto configurará automaticamente um filtro WMI para garantir que o GPO do cliente do DirectAccess seja aplicado apenas a computadores móveis nos grupos de segurança especificados. O administrador do Acesso Remoto requer permissões para criar ou modificar os filtros WMI da política de grupo para habilitar essa configuração.  
  
2.  Quais grupos de segurança conterão os computadores cliente DirectAccess?  
  
    As configurações do DirectAccess estão contidas no objeto de diretiva de grupo (GPO) de cliente do DirectAccess. O GPO é aplicado a computadores que fazem parte dos grupos de segurança que você especificou no Assistente de configuração do cliente do DirectAccess. Você pode especificar grupos de segurança que estão contidos em qualquer domínio com suporte.
  
    Antes de configurar o acesso remoto, você precisará criar os grupos de segurança. Você pode adicionar computadores ao grupo de segurança depois de concluir a implantação do acesso remoto. No entanto, se você adicionar computadores cliente que residem em um domínio diferente do grupo de segurança, o GPO do cliente não será aplicado a esses clientes. Por exemplo, se você criou SG1 no domínio A para clientes DirectAccess e posteriormente, você adicionar clientes do domínio B a esse grupo, o GPO do cliente será não ser aplicado aos clientes no domínio B.  
  
    Para evitar esse problema, crie um novo grupo de segurança de cliente para cada domínio que contém computadores cliente. Como alternativa, se você não quiser criar um novo grupo de segurança, execute as **Add-DAClient** cmdlet do Windows PowerShell com o nome do novo GPO para o novo domínio.  
  
3.  Quais configurações serão definidas para o Assistente de conectividade de rede do DirectAccess?  
  
    O Assistente de conectividade de rede do DirectAccess é executado em computadores cliente e fornece informações adicionais sobre a conexão do DirectAccess para os usuários finais. No Assistente de configuração do cliente do DirectAccess, você pode configurar o seguinte:  
  
    -   **Verificadores de conectividade**  
  
        Uma sonda de web padrão é criada para os clientes validarem a conectividade com a rede interna. O nome padrão é `https://directaccess-WebProbeHost.<domain_name>`. O nome deverá ser registrado manualmente no DNS. Você pode criar outros verificadores de conectividade que usam outros endereços da web via HTTP ou PING. Uma entrada de DNS deverá existir para cada verificador de conectividade.  
  
    -   **Ajuda do endereço de email do suporte técnico**  
  
        Se os usuários finais ter problemas de conectividade do DirectAccess, eles podem enviar um email que contém informações de diagnóstico para o administrador de acesso remoto, que pode solucionar o problema.  
  
    -   **Nome da conexão do DirectAccess**  
  
        Você pode especificar um nome de conexão do DirectAccess para ajudar os usuários finais a identificar a conexão do DirectAccess em seu computador.  
  
    -   **Permitir que os clientes do DirectAccess usar a resolução de nome local**  
  
        Os clientes exigem um meio de resolver nomes localmente. Se você permitir que os clientes do DirectAccess usem a resolução de nome local, os usuários finais poderão usar os servidores de DNS local para resolver os nomes. Quando os usuários finais opta por usar servidores DNS locais para a resolução de nome, o DirectAccess não envia solicitações de resolução de nomes de rótulo único para o servidor DNS corporativo interno. Ele usa resolução de nomes locais em vez disso (usando o Link-Local Multicast nome LLMNR (resolução) e o NetBios nos protocolos TCP/IP).  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>Planejar uma estratégia de implantação do servidor de acesso remoto  
As decisões que você precisa fazer quando você estiver planejando implantar o servidor de acesso remoto incluem:  
  
-   **Topologia de rede**  
  
    Há duas topologias disponíveis ao implantar um servidor de acesso remoto:  
  
    -   **Dois adaptadores**: Com dois adaptadores de rede, acesso remoto pode ser configurado com um adaptador de rede conectado diretamente à Internet e o outro conectado à rede interna. Ou, Alternativamente, o servidor está instalado atrás de um dispositivo de borda, como um firewall ou um roteador. Essa rede de uma configuração de adaptador está conectado à rede de perímetro e o outro é conectado à rede interna.  
  
    -   **Adaptador de rede único**: Nessa configuração, o acesso remoto do servidor é instalado atrás de um dispositivo de borda, como um firewall ou um roteador. O adaptador de rede é conectado à rede interna.  

-   **Adaptadores de rede**  
  
    O Assistente de instalação de servidor de acesso remoto detecta automaticamente os adaptadores de rede são configurados no servidor de acesso remoto. Verifique se os adaptadores corretos foram selecionados.  
  
-   **Certificado IP-HTTPS**  
  
    O Assistente de configuração de servidor de acesso remoto detecta automaticamente um certificado adequado para a conexão IP-HTTPS. O nome do assunto do certificado selecionado deve ser correspondente ao endereço ConnectTo. Se você estiver usando certificados autoassinados, você pode optar por usar um certificado que é criado automaticamente pelo servidor de acesso remoto.  
  
-   **Prefixos IPv6**  
  
    Se o Assistente de configuração do servidor de acesso remoto detectar que o IPv6 foi implantado nos adaptadores de rede, ele preencherá automaticamente os prefixos de IPv6 para a rede interna, outro para a atribuição dos computadores cliente do DirectAccess e outro para a atribuição aos computadores cliente do VPN. Se os prefixos gerados automaticamente não estiverem corretos para sua infraestrutura IPv6 ou ISATAP nativa, altere-os manualmente.  
  
-   **Autenticação**  
  
    Você pode escolher um dos seguintes métodos para autenticar os clientes DirectAccess para o servidor de acesso remoto:  
  
    -   **Autenticação de usuário**: Você pode habilitar os usuários para autenticação com credenciais do Active Directory ou com autenticação em dois fatores.  
  
    -   **Autenticação de computador**: Você pode configurar a autenticação de computador para usar certificados. Ou o servidor de acesso remoto pode agir como um proxy para a autenticação Kerberos sem a necessidade de certificados. 
  
    -   **Os clientes do Windows 7** por padrão, os computadores cliente que executam o Windows 7 não podem se conectar a uma implantação de acesso remoto executando o Windows Server 2012. Se você tiver clientes que executam o Windows 7 em sua organização que requerem acesso remoto aos recursos internos, você pode permitir que eles se conectem. Qualquer computador cliente que poderá receber acesso aos recursos internos deverá ser membro de um grupo de segurança especificado no Assistente de configuração do cliente do DirectAccess.  
  
        > [!NOTE]  
        > Permitindo que os clientes que executam o Windows 7 para se conectar usando o DirectAccess requer que você utilize autenticação de certificado do computador.  
  
-   **Configuração de VPN**  
  
    Antes de configurar o acesso remoto, decida se você pretende fornecer acesso VPN para clientes remotos. Você deve fornecer acesso VPN se você tiver computadores cliente em sua organização que não dão suporte a conectividade do DirectAccess (por exemplo, eles não são gerenciados ou executarem um sistema operacional para que o DirectAccess não é suportado). O Assistente de instalação de servidor de acesso remoto permite que você configure como os endereços IP são atribuídos (usando o DHCP ou de um pool de endereços estáticos) e como os clientes VPN são autenticados (por meio do Active Directory ou um servidor RADIUS).  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>Planejar configurações de servidores de infra-estrutura  
Acesso remoto requer três tipos de servidores de infraestrutura:  
  
-   **Servidor de local de rede**  
  
-   **Servidores DNS** 
  
-   **Servidores de gerenciamento** 
  
## <a name="see-also"></a>Consulte também  
  
-   [Etapa 1: Planejar a infraestrutura de acesso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


