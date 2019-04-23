---
title: Etapa 2 planejar a implantação básica do DirectAccess
description: Este tópico faz parte do guia de implantar um único servidor DirectAccess usando o Introdução ao Assistente para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8c21b7fa62246170caeb07cb5865c1ff311e0f09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848747"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>Etapa 2 planejar a implantação básica do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Após planejar a infraestrutura do DirectAccess, a próxima etapa na implantação do DirectAccess em um único servidor com configurações básicas é planejar as configurações para o Assistente de Introdução.  
  
|Tarefa|Descrição|  
|----|--------|  
|Planejando a implantação do cliente|Por padrão, o Assistente de Introdução implanta o DirectAccess em todos os laptops e notebooks no domínio, aplicando um filtro WMI para o GPO de configurações do cliente|  
|Planejar a implantação de servidor do DirectAccess|Planeje como implantar o servidor do DirectAccess.|  
  
## <a name="bkmk_2_1_client"></a>Planejar a implantação de cliente  
Há duas decisões a serem tomadas ao planejar a implantação do cliente:  
  
1.  O DirectAccess estará disponível somente para computadores móveis, ou para qualquer computador?  
  
    Quando você configura os clientes do DirectAccess no Assistente de Introdução, você pode optar por permitir somente computadores móveis nos grupos de segurança especificados sejam conectados usando DirectAccess. Se você restringir o acesso a computadores móveis, o DirectAccess configura automaticamente um filtro WMI para garantir que o cliente do DirectAccess que GPO é aplicado somente a computadores móveis nos grupos de segurança especificados. O administrador do DirectAccess requer permissões para criar ou modificar filtros WMI de diretiva de grupo para habilitar essa configuração.  
  
2.  Quais grupos de segurança conterão os computadores cliente DirectAccess?  
  
    As configurações do DirectAccess estão contidas no GPO do cliente do DirectAccess. O GPO é aplicado a computadores que fazem parte dos grupos de segurança que você especificar no Assistente do guia de Introdução. Você pode especificar grupos de segurança contidos em qualquer domínio com suporte. Antes de configurar o DirectAccess, os grupos de segurança devem ser criados. Você pode adicionar computadores ao grupo de segurança depois de concluir a implantação do DirectAccess, mas observe que se você adicionar computadores cliente que residem em um domínio diferente para o grupo de segurança, o GPO do cliente não será aplicado a esses clientes. Por exemplo, se você criou SG1 no domínio A para clientes do DirectAccess, e posteriormente adicionou clientes do domínio B a esse grupo, o GPO do cliente não será aplicado a clientes no domínio B. Para evitar esse problema, crie um novo grupo de segurança de cliente para cada domínio que contém computadores cliente. Também, se você não deseja criar um novo grupo de segurança, execute o cmdlet Add-DAClient com o nome do novo GPO do novo domínio.  
  
## <a name="bkmk_2_2_server"></a>Planejar a implantação de servidor do DirectAccess  
Há uma série de decisões a serem tomadas ao planejar a implantação de seu servidor do DirectAccess:  
  
-   **Topologia de rede** -existem duas topologias disponíveis ao implantar um servidor DirectAccess:  
  
    -   **Dois adaptadores** -com dois adaptadores de rede, o DirectAccess pode ser configurado com um adaptador de rede conectado diretamente à Internet e o outro é conectado à rede interna. Outra alternativa é o servidor ser instalado por trás de um dispositivo de borda, como um firewall ou um roteador. Nessa configuração, um adaptador de rede é conectado à rede de perímetro e o outro é conectado à rede interna.  
  
    -   **Adaptador de rede único** – nessa configuração do DirectAccess server é instalado atrás de um dispositivo de borda como um firewall ou roteador. O adaptador de rede é conectado à rede interna.  
  
-   **Adaptadores de rede** -Assistente do DirectAccess detecta automaticamente os adaptadores de rede configurados no servidor DirectAccess. Você pode tornar-se de que os adaptadores corretos foram selecionados do **revisão** página.  
  
-   **Certificado IP-HTTPS** -como não há nenhum necessária nessa implantação de PKI, o assistente provisiona automaticamente os certificados autoassinados para IP-HTTPS e o servidor de local de rede (se nenhum certificado estiver presente) e habilita automaticamente Proxy de Kerberos. O assistente também permite NAT64 e DNS64 para conversão de protocolo no ambiente somente com IPv4. Depois que o assistente concluir com sucesso aplicando a configuração, clique em **fechar**.  
  
-   **Os clientes do Windows 7** -não é possível habilitar o suporte para clientes do Windows 7 do Assistente de Introdução. Isso pode ser habilitado do Assistente de configuração avançada. Para obter mais detalhes, consulte [implantar um único servidor DirectAccess com configurações avançadas](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
-   **Configuração de VPN** -antes de configurar o DirectAccess, decida se você pretende fornecer acesso VPN para clientes remotos. Você deve fornecer acesso VPN se você tiver computadores cliente em sua organização que não dão suporte a conectividade do DirectAccess (seja porque eles não são gerenciados ou executam um sistema operacional para que o DirectAccess não é suportado). O Assistente de Introdução configura a atribuição de endereço IP de VPN usando o DHCP e configura os clientes VPN a ser autenticado usando o Active Directory.  
  
-   **Forçar túnel** – se você planeja usar o túnel forçado, ou poderá adicioná-lo no futuro, você deve usar [implantar um único servidor DirectAccess com configurações avançadas](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) para implantar uma configuração de dois túneis. Devido a considerações de segurança, criar túneis à força em uma configuração de túnel único não é suportado.  
  
## <a name="BKMK_Links"></a>Etapa anterior  
  
-   [Etapa 1: Planejar a infraestrutura básica do DirectAccess](da-basic-plan-s1-infrastructure.md)  
  


