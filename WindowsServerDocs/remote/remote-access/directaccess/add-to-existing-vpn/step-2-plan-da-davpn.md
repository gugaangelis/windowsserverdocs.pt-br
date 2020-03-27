---
title: Etapa 2 planejar a implantação do DirectAccess
description: Este tópico faz parte do guia adicionar o DirectAccess a uma implantação de VPN (acesso remoto) existente para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 72b5b2af-6925-41e0-a3f9-b8809ed711d1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 865f9ffd3eed3ce145364c227845af097194416e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309242"
---
# <a name="step-2-plan-the-directaccess-deployment"></a>Etapa 2 planejar a implantação do DirectAccess

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Após planejar a infraestrutura de Acesso Remoto, a próxima etapa para habilitar o DirectAccess é planejar as configurações do Assistente para Habilitar DirectAccess.  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|----|--------|  
|Planejando a implantação do cliente|Planeje como permitir que computadores cliente sejam conectados usando DirectAccess. Decida quais computadores gerenciados serão configurados como clientes do DirectAccess.|  
|Planejando a implantação do servidor de Acesso Remoto|Planeje como implantar o servidor de Acesso Remoto.|  
  
## <a name="planning-for-client-deployment"></a><a name="bkmk_2_1_client"></a>Planejando a implantação do cliente  
Há duas decisões a serem tomadas ao planejar a implantação do cliente:  
  
-   O DirectAccess estará disponível somente para computadores móveis, ou para qualquer computador?  
  
    Ao configurar clientes do DirectAccess no assistente para Habilitar DirectAccess, você pode escolher permitir que somente computadores móveis nos grupos de segurança especificados sejam conectados usando DirectAccess. Se você restringir o acesso a computadores móveis, o Acesso Remoto configurará automaticamente um filtro WMI para garantir que o GPO do cliente do DirectAccess seja aplicado apenas a computadores móveis nos grupos de segurança especificados. O administrador do Acesso Remoto requer permissões para criar ou modificar os filtros WMI da política de grupo para habilitar essa configuração.  
  
-   Quais grupos de segurança conterão os computadores cliente DirectAccess?  
  
    As configurações do DirectAccess estão contidas no GPO do cliente do DirectAccess. O GPO é aplicado a computadores que fazem parte dos grupos de segurança especificados no assistente para Habilitar DirectAccess. Você pode especificar grupos de segurança contidos em qualquer domínio com suporte. Antes de configurar o Acesso Remoto, os grupos de segurança devem ser criados. Você pode adicionar computadores ao grupo de segurança depois de concluir a implantação do Acesso Remoto, mas observe que, se adicionar computadores cliente que residem em um domínio diferente ao grupo de segurança, o GPO do cliente não será aplicado a esses clientes. Por exemplo, se você criou SG1 no domínio A para clientes do DirectAccess, e posteriormente adicionou clientes do domínio B a esse grupo, o GPO do cliente não será aplicado a clientes no domínio B. Para evitar esse problema, crie um novo grupo de segurança de cliente para cada domínio que contém computadores cliente. Também, se você não deseja criar um novo grupo de segurança, execute o cmdlet Add-DAClient com o nome do novo GPO do novo domínio.  
  
## <a name="planning-for-remote-access-server-deployment"></a><a name="bkmk_2_2_server"></a>Planejando a implantação do servidor de acesso remoto  
Há diversas decisões a serem tomadas ao se planejar a implantação de seu servidor de Acesso Remoto:  
  
-   **Topologia de rede**– há duas topologias disponíveis ao implantar um servidor de acesso remoto:  
  
    -   **Dois adaptadores**– com dois adaptadores de rede, o acesso remoto pode ser configurado com um adaptador de rede conectado diretamente à Internet e o outro está conectado à rede interna. Outra alternativa é o servidor ser instalado por trás de um dispositivo de borda, como um firewall ou um roteador. Nessa configuração, um adaptador de rede é conectado à rede de perímetro e o outro é conectado à rede interna.  
  
    -   **Adaptador de rede único**– nessa configuração, o servidor de acesso remoto é instalado atrás de um dispositivo de borda, como um firewall ou um roteador. O adaptador de rede é conectado à rede interna.  
  
-   **Adaptadores de rede**– o assistente habilitar DirectAccess detecta automaticamente os adaptadores de rede configurados no servidor de acesso remoto, com base nas interfaces usadas pela VPN. Verifique se os adaptadores corretos foram selecionados.  
  
-   **Connectto**-os computadores cliente usam o endereço connectto para se conectar ao servidor de acesso remoto. O endereço que você escolher deve corresponder ao nome da entidade do certificado IP-HTTPS implantado para a conexão IP-HTTPS e deve estar disponível no DNS público. Consulte Planejando certificados do site para IP-HTTPS.  
  
-   **Certificado IP-HTTPS**-se a VPN SSTP estiver configurada, o assistente habilitar DirectAccess selecionará o certificado usado pelo SSTP para IP-HTTPS. Se o SSTP VPN não estiver configurado, o assistente tentará ver se um certificado para IP-HTTPS foi configurado. Caso não tenha sido, ele provisionará automaticamente certificados autoassinados para IP-HTTPS. O assistente habilita automaticamente autenticação Kerberos. O assistente também habilitará NAT64 e DNS64 para conversão de protocolos no ambiente somente IPv4.  
  
-   **Prefixos IPv6**– se o assistente detectar que o IPv6 foi implantado nos adaptadores de rede, ele criará automaticamente prefixos IPv6 para a rede interna, um prefixo IPv6 para atribuir a computadores cliente DirectAccess e um prefixo IPv6 para atribuir a computadores cliente VPN. Se os prefixos gerados automaticamente não estiverem corretos para sua infraestrutura IPv6 ou ISATAP nativa, altere-os manualmente. Consulte 1.1 Planejando topologia e configurações de rede e servidor.  
  
-   **Clientes do Windows 7**-por padrão, os computadores cliente do Windows 7 não podem se conectar a uma implantação de acesso remoto do windows Server 2012. Se você tiver computadores cliente do Windows 7 em sua organização que exigem acesso remoto a recursos internos, você poderá permitir que eles se conectem. Qualquer computador cliente ao qual você deseja permitir acesso a recursos internos deve ser membro de um grupo de segurança especificado no assistente para Habilitar DirectAccess.  
  
    > [!NOTE]
    > Permitir que computadores cliente com Windows 7 se conectem usando o DirectAccess exige que você use a autenticação de certificado do computador.
  
-   **Autenticação**– o assistente habilitar DirectAccess usa Active Directory para autenticar as credenciais do usuário. Para implantar a autenticação de dois fatores, consulte [Implantar o Acesso Remoto com a autenticação OTP](../../ras/otp/Deploy-RA-OTP.md).  
  

  


