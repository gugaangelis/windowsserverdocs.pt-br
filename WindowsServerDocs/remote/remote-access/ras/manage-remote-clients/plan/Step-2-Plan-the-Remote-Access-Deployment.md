---
title: Etapa 2 planejar a implantação de acesso remoto
description: Este tópico faz parte do guia gerenciar clientes DirectAccess remotamente no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: lizross
author: eross-msft
ms.openlocfilehash: befd517f8e00548524dc9bf9c328d63034c653be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860569"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>Etapa 2 planejar a implantação de acesso remoto

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Depois de planejar a infraestrutura que você pretende usar para configurar seu único servidor de acesso remoto para o gerenciamento remoto de clientes DirectAccess, você estará pronto para planejar as configurações que o assistente de configuração de acesso remoto usará.  
  
> [!NOTE]  
> Antes de continuar com essas tarefas, consulte [etapa 1: planejar a infraestrutura de acesso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|----|--------|  
|[Planejar uma estratégia de implantação de cliente](#plan-a-client-deployment-strategy)|Decida quais computadores gerenciados serão configurados como clientes do DirectAccess.|  
|[Planejar uma estratégia de implantação de servidor de acesso remoto](#plan-a-remote-access-server-deployment-strategy)|Planeje como implantar o servidor de Acesso Remoto.|  
|[Planejar as configurações dos servidores de infraestrutura](#plan-the-infrastructure-servers-configurations)|Planeje os servidores de infraestrutura em sua implantação de acesso remoto, incluindo o servidor de local de rede do DirectAccess, servidores DNS e servidores de gerenciamento do DirectAccess.|  
  
## <a name="plan-a-client-deployment-strategy"></a>Planejar uma estratégia de implantação de cliente  
Existem três decisões que precisam ser tomadas ao planejar a implantação do cliente:  
  
1.  O DirectAccess estará disponível somente para computadores móveis ou para cada computador em um grupo de segurança especificado?  
  
    Ao configurar clientes DirectAccess no assistente de instalação do cliente DirectAccess, você pode optar por permitir que somente computadores móveis em grupos de segurança especificados se conectem ao servidor usando o DirectAccess. Se você restringir o acesso a computadores móveis, o Acesso Remoto configurará automaticamente um filtro WMI para garantir que o GPO do cliente do DirectAccess seja aplicado apenas a computadores móveis nos grupos de segurança especificados. O administrador do Acesso Remoto requer permissões para criar ou modificar os filtros WMI da política de grupo para habilitar essa configuração.  
  
2.  Quais grupos de segurança conterão os computadores cliente DirectAccess?  
  
    As configurações do DirectAccess estão contidas no objeto de Política de Grupo do cliente do DirectAccess (GPO). O GPO é aplicado a computadores que fazem parte dos grupos de segurança que você especificou no Assistente de configuração do cliente do DirectAccess. Você pode especificar grupos de segurança que estão contidos em qualquer domínio com suporte.
  
    Antes de configurar o acesso remoto, você precisa criar os grupos de segurança. Você pode adicionar computadores ao grupo de segurança depois de concluir a implantação de acesso remoto. No entanto, se você adicionar computadores cliente que residem em um domínio diferente do grupo de segurança, o GPO do cliente não será aplicado a esses clientes. Por exemplo, se você criou SG1 no domínio A para clientes do DirectAccess e posteriormente adicionar clientes do domínio B a esse grupo, o GPO do cliente não será aplicado aos clientes no domínio B.  
  
    Para evitar esse problema, crie um novo grupo do Client Security para cada domínio que contenha computadores cliente. Como alternativa, se você não quiser criar um novo grupo de segurança, execute o cmdlet **Add-DAClient** do Windows PowerShell com o nome do novo GPO para o novo domínio.  
  
3.  Quais configurações você vai configurar para o assistente de conectividade de rede do DirectAccess?  
  
    O assistente de conectividade de rede do DirectAccess é executado em computadores cliente e fornece informações adicionais sobre a conexão do DirectAccess para os usuários finais. No Assistente de configuração do cliente do DirectAccess, você pode configurar o seguinte:  
  
    -   **Verificadores de conectividade**  
  
        Uma sonda de web padrão é criada para os clientes validarem a conectividade com a rede interna. O nome padrão é `https://directaccess-WebProbeHost.<domain_name>`. O nome deverá ser registrado manualmente no DNS. Você pode criar outros verificadores de conectividade que usam outros endereços da Web via HTTP ou PING. Uma entrada de DNS deverá existir para cada verificador de conectividade.  
  
    -   **Endereço de email do suporte técnico**  
  
        Se os usuários finais tiverem problemas de conectividade do DirectAccess, eles poderão enviar um email que contém informações de diagnóstico para o administrador de acesso remoto, que pode solucionar o problema.  
  
    -   **Nome da conexão do DirectAccess**  
  
        Você pode especificar um nome de conexão do DirectAccess para ajudar os usuários finais a identificar a conexão do DirectAccess em seu computador.  
  
    -   **Permitir que os clientes DirectAccess usem a resolução de nomes locais**  
  
        Os clientes exigem um meio de resolver nomes localmente. Se você permitir que os clientes do DirectAccess usem a resolução de nome local, os usuários finais poderão usar os servidores de DNS local para resolver os nomes. Quando os usuários finais optam por usar servidores DNS locais para a resolução de nomes, o DirectAccess não envia solicitações de resolução para nomes de rótulo único para o servidor DNS corporativo interno. Em vez disso, ele usa a resolução de nome local (usando o LLMNR) e os protocolos NetBios sobre TCP/IP.  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>Planejar uma estratégia de implantação de servidor de acesso remoto  
As decisões que você precisa fazer ao planejar a implantação do servidor de acesso remoto incluem:  
  
-   **Topologia de rede**  
  
    Há duas topologias disponíveis ao implantar um servidor de acesso remoto:  
  
    -   **Dois adaptadores**: com dois adaptadores de rede, o acesso remoto pode ser configurado com um adaptador de rede conectado diretamente à Internet e o outro conectado à rede interna. Ou, como alternativa, o servidor é instalado atrás de um dispositivo de borda, como um firewall ou um roteador. Nessa configuração, um adaptador de rede é conectado à rede de perímetro e o outro é conectado à rede interna.  
  
    -   **Adaptador de rede único**: nessa configuração, o servidor de acesso remoto é instalado atrás de um dispositivo de borda, como um firewall ou um roteador. O adaptador de rede é conectado à rede interna.  

-   **Adaptadores de rede**  
  
    O assistente de instalação do servidor de acesso remoto detecta automaticamente os adaptadores de rede configurados no servidor de acesso remoto. Verifique se os adaptadores corretos foram selecionados.  
  
-   **Certificado IP-HTTPS**  
  
    O Assistente de configuração de servidor de acesso remoto detecta automaticamente um certificado adequado para a conexão IP-HTTPS. O nome do assunto do certificado selecionado deve ser correspondente ao endereço ConnectTo. Se você estiver usando certificados autoassinados, poderá optar por usar um certificado criado automaticamente pelo servidor de acesso remoto.  
  
-   **Prefixos IPv6**  
  
    Se o Assistente de configuração do servidor de acesso remoto detectar que o IPv6 foi implantado nos adaptadores de rede, ele preencherá automaticamente os prefixos de IPv6 para a rede interna, outro para a atribuição dos computadores cliente do DirectAccess e outro para a atribuição aos computadores cliente do VPN. Se os prefixos gerados automaticamente não estiverem corretos para sua infraestrutura IPv6 ou ISATAP nativa, altere-os manualmente.  
  
-   **Autenticação**  
  
    Você pode escolher um dos seguintes métodos para autenticar clientes do DirectAccess no servidor de acesso remoto:  
  
    -   **Autenticação de usuário**: você pode habilitar usuários para autenticar com Active Directory credenciais ou com a autenticação de dois fatores.  
  
    -   **Autenticação do computador**: você pode configurar a autenticação do computador para usar certificados. Ou o servidor de acesso remoto pode atuar como um proxy para autenticação Kerberos sem a necessidade de certificados. 
  
    -   **Clientes do Windows 7** Por padrão, os computadores cliente que executam o Windows 7 não podem se conectar a uma implantação de acesso remoto que executa o Windows Server 2012. Se você tiver clientes que executam o Windows 7 em sua organização que exigem acesso remoto a recursos internos, você poderá permitir que eles se conectem. Qualquer computador cliente que poderá receber acesso aos recursos internos deverá ser membro de um grupo de segurança especificado no Assistente de configuração do cliente do DirectAccess.  
  
        > [!NOTE]  
        > Permitir que clientes que executam o Windows 7 se conecte usando o DirectAccess exige que você use a autenticação de certificado do computador.  
  
-   **Configuração de VPN**  
  
    Antes de configurar o acesso remoto, decida se você vai fornecer acesso VPN a clientes remotos. Você deve fornecer acesso VPN se tiver computadores cliente em sua organização que não dão suporte à conectividade do DirectAccess (por exemplo, eles não são gerenciados ou executam um sistema operacional para o qual o DirectAccess não tem suporte). O assistente de instalação do servidor de acesso remoto permite que você configure como os endereços IP são atribuídos (usando DHCP ou de um pool de endereços estáticos) e como os clientes VPN são autenticados (usando Active Directory ou um servidor RADIUS).  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>Planejar as configurações dos servidores de infraestrutura  
O acesso remoto requer três tipos de servidores de infraestrutura:  
  
-   **Servidor de local de rede**  
  
-   **Servidores DNS** 
  
-   **Servidores de gerenciamento** 
  
## <a name="see-also"></a>Consulte também  
  
-   [Etapa 1: planejar a infraestrutura de acesso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


