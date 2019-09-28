---
title: Etapa 2 planejar a implantação básica do DirectAccess
description: Este tópico faz parte do guia implantar um único servidor DirectAccess usando o assistente de Introdução para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3009b6002d9d4cd116795c46305ff02fda02ef63
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388486"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>Etapa 2 planejar a implantação básica do DirectAccess

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Depois de planejar a infraestrutura do DirectAccess, a próxima etapa na implantação do DirectAccess em um único servidor com as configurações básicas é planejar as configurações para o assistente de Introdução.  
  
|Tarefa|Descrição|  
|----|--------|  
|Planejando a implantação do cliente|Por padrão, o assistente de Introdução implanta o DirectAccess em todos os laptops e computadores notebook no domínio aplicando um filtro WMI ao GPO configurações do cliente|  
|Planejando a implantação do servidor DirectAccess|Planeje como implantar o servidor do DirectAccess.|  
  
## <a name="bkmk_2_1_client"></a>Planejando a implantação do cliente  
Há duas decisões a serem tomadas ao planejar a implantação do cliente:  
  
1.  O DirectAccess estará disponível somente para computadores móveis, ou para qualquer computador?  
  
    Ao configurar clientes DirectAccess no assistente de Introdução, você pode optar por permitir que somente computadores móveis nos grupos de segurança especificados se conectem usando o DirectAccess. Se você restringir o acesso a computadores móveis, o DirectAccess configura automaticamente um filtro WMI para garantir que o GPO do cliente DirectAccess seja aplicado somente a computadores móveis nos grupos de segurança especificados. O administrador do DirectAccess requer permissões para criar ou modificar filtros WMI da política de grupo para habilitar essa configuração.  
  
2.  Quais grupos de segurança conterão os computadores cliente DirectAccess?  
  
    As configurações do DirectAccess estão contidas no GPO do cliente do DirectAccess. O GPO é aplicado a computadores que fazem parte dos grupos de segurança que você especifica no assistente de Introdução. Você pode especificar grupos de segurança contidos em qualquer domínio com suporte. Antes de configurar o DirectAccess, os grupos de segurança devem ser criados. Você pode adicionar computadores ao grupo de segurança depois de concluir a implantação do DirectAccess, mas observe que, se você adicionar computadores cliente que residem em um domínio diferente ao grupo de segurança, o GPO do cliente não será aplicado a esses clientes. Por exemplo, se você criou SG1 no domínio A para clientes do DirectAccess, e posteriormente adicionou clientes do domínio B a esse grupo, o GPO do cliente não será aplicado a clientes no domínio B. Para evitar esse problema, crie um novo grupo de segurança de cliente para cada domínio que contém computadores cliente. Também, se você não deseja criar um novo grupo de segurança, execute o cmdlet Add-DAClient com o nome do novo GPO do novo domínio.  
  
## <a name="bkmk_2_2_server"></a>Planejando a implantação do servidor DirectAccess  
Há várias decisões a serem tomadas ao planejar a implantação do servidor DirectAccess:  
  
-   **Topologia de rede** – há duas topologias disponíveis ao implantar um servidor DirectAccess:  
  
    -   **Dois adaptadores** – com dois adaptadores de rede, o DirectAccess pode ser configurado com um adaptador de rede conectado diretamente à Internet e o outro está conectado à rede interna. Outra alternativa é o servidor ser instalado por trás de um dispositivo de borda, como um firewall ou um roteador. Nessa configuração, um adaptador de rede é conectado à rede de perímetro e o outro é conectado à rede interna.  
  
    -   **Adaptador de rede único** – nessa configuração, o servidor DirectAccess é instalado atrás de um dispositivo de borda, como um firewall ou um roteador. O adaptador de rede é conectado à rede interna.  
  
-   **Adaptadores de rede** : o assistente do DirectAccess detecta automaticamente os adaptadores de rede configurados no servidor DirectAccess. Você pode garantir que os adaptadores corretos sejam selecionados na página **revisão** .  
  
-   **Certificado IP-HTTPS** -como não há nenhuma PKI necessária nessa implantação, o assistente provisiona automaticamente certificados autoassinados para IP-HTTPS e o servidor de local de rede (se nenhum certificado estiver presente) e habilita automaticamente o Kerberos acionista. O assistente também habilita o NAT64 e o DNS64 para conversão de protocolo no ambiente somente IPv4. Depois que o assistente for concluído com êxito aplicando a configuração, clique em **fechar**.  
  
-   **Clientes do Windows 7** – não é possível habilitar o suporte para clientes do Windows 7 por meio do assistente de introdução. Isso pode ser habilitado no assistente de configuração avançada. Para obter mais detalhes, consulte [implantar um único servidor DirectAccess com configurações avançadas](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
-   **Configuração de VPN** -antes de configurar o DirectAccess, decida se você vai fornecer acesso VPN a clientes remotos. Você deve fornecer acesso VPN se tiver computadores cliente em sua organização que não dão suporte à conectividade do DirectAccess (porque eles não são gerenciados ou executam um sistema operacional para o qual o DirectAccess não tem suporte). O assistente de Introdução configura a atribuição de endereço IP VPN usando DHCP e configura os clientes VPN para serem autenticados usando Active Directory.  
  
-   **Túnel forçado** – se você planeja usar o túnel forçado ou pode adicioná-lo no futuro, use [implantar um único servidor DirectAccess com configurações avançadas](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) para implantar uma configuração de dois túneis. Devido a considerações de segurança, não há suporte para o túnel forçado em uma única configuração de túnel.  
  
## <a name="BKMK_Links"></a>Etapa anterior  
  
-   [Etapa 1: Planejar a infraestrutura do DirectAccess Básico](da-basic-plan-s1-infrastructure.md)  
  


