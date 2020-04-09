---
title: Etapa 3 planejar uma implantação de cluster com balanceamento de carga
description: Este tópico faz parte do guia implantar o acesso remoto em um cluster no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 7540c17b-81de-47de-a04f-3247afa26f70
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 28a255031e9168105b285dbece1c9230d1ab20a9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855229"
---
# <a name="step-3-plan-a-load-balanced-cluster-deployment"></a>Etapa 3 planejar uma implantação de cluster com balanceamento de carga

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

A próxima etapa é planejar a configuração de balanceamento de carga e a implantação de cluster.  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|----|--------|  
|balanceamento de carga do plano 3,1|Decida se deseja usar o NLB (balanceamento de carga de rede) do Windows ou um ELB (balanceador de carga externo).|  
|IP do plano 3,2-HTTPS|Se um certificado autoassinado não for usado, o servidor de acesso remoto precisará de um certificado SSL em cada servidor no cluster para autenticar conexões IP-HTTPS.|  
|3,3 plano para conexões de cliente VPN|Observe os requisitos para conexões de cliente VPN.|  
|3,4 planejar o servidor do local de rede|Se o site do servidor do local de rede estiver hospedado no servidor de acesso remoto e um certificado autoassinado não for usado, verifique se cada servidor no cluster tem um certificado de servidor para autenticar a conexão com o site.|  
  
## <a name="31-plan-load-balancing"></a><a name="bkmk_2_1_Plan_LB"></a>balanceamento de carga do plano 3,1  
O acesso remoto pode ser implantado em um único servidor ou em um cluster de servidores de acesso remoto. O tráfego para o cluster pode ter balanceamento de carga para fornecer alta disponibilidade e escalabilidade para clientes DirectAccess. Há duas opções de balanceamento de carga:  
  
-   **Windows NLB**-o NLB do Windows é um recurso do Windows Server. Para usá-lo, você não precisa de hardware adicional, pois todos os servidores no cluster são responsáveis por gerenciar a carga de tráfego. O NLB do Windows dá suporte a um máximo de oito servidores em um cluster de acesso remoto.  
  
-   **Balanceador de carga externo**-usar um balanceador de carga externo requer hardware externo para gerenciar a carga de tráfego entre os servidores de cluster de acesso remoto. Além disso, o uso de um balanceador de carga externo dá suporte a um máximo de 32 servidores de acesso remoto em um cluster. Alguns pontos para ter em mente ao configurar o balanceamento de carga externo são:  
  
    -   O administrador deve garantir que os IPs virtuais configurados por meio do assistente de balanceamento de carga de acesso remoto sejam usados nos balanceadores de carga externos (como F5 sistema de Gerenciador de tráfego local de Big-IP). Quando o balanceamento de carga externo estiver habilitado, os endereços IP nas interfaces externas e internas serão promovidos para endereços IP virtuais e precisarão ser inseridos nos balanceadores de carga. Isso é feito para que o administrador não precise alterar a entrada DNS para o nome público da implantação do cluster. Além disso, os pontos de extremidade de túnel IPsec são derivados dos IPs de servidor. Se o administrador fornece IPs virtuais separados, o cliente não poderá se conectar ao servidor. Consulte o exemplo para configurar o DirectAccess com balanceamento de carga externo no exemplo de configuração de Load Balancer externo 3.1.1.  
  
    -   Muitos balanceadores de carga externos (incluindo F5) não dão suporte ao balanceamento de carga de 6to4 e ISATAP. Se o servidor de acesso remoto for um roteador ISATAP, a função ISATAP deverá ser movida para um computador diferente. Além disso, quando a função ISATAP está em um computador diferente, os servidores DirectAccess devem ter conectividade IPv6 nativa com o roteador ISATAP. Observe que essa conectividade deve estar presente antes de configurar o DirectAccess.  
  
    -   Para o balanceamento de carga externo, se Teredo precisar ser usado, todos os servidores de acesso remoto deverão ter dois endereços IPv4 públicos consecutivos como endereços IP dedicados. Os IPs virtuais do cluster também devem ter dois endereços IPv4 públicos consecutivos. Isso não é verdadeiro para o NLB do Windows em que apenas os IPs virtuais do cluster devem ter dois endereços IPv4 públicos consecutivos. Se o Teredo não for usado, dois endereços IP consecutivos não serão necessários.  
  
    -   O administrador pode alternar do NLB do Windows para o balanceador externo de carga e vice-versa. Observe que o administrador não poderá alternar do balanceador externo de carga para o NLB do Windows se ele tiver mais de 8 servidores na implantação do balanceador de carga externo.  
  
### <a name="311-external-load-balancer-configuration-example"></a><a name="ELBConfigEx"></a>exemplo de configuração de Load Balancer externa 3.1.1  
Esta seção descreve as etapas de configuração para habilitar um balanceador de carga externo em uma nova implantação de acesso remoto. Ao usar um balanceador de carga externo, o cluster de acesso remoto pode ser semelhante à figura abaixo, em que os servidores de acesso remoto estão conectados à rede corporativa por meio de um balanceador de carga na rede interna e à Internet por meio de um balanceador de carga conectado à rede externa:  
  
![Exemplo de configuração de Load Balancer externo](../../../../media/Step-3-Plan-a-Load-Balanced-Cluster-Deployment/ELBDiagram-URA_Enterprise_NLB-.png)  
  
##### <a name="planning-information"></a>Informações de planejamento  
  
1.  Os VIPs externos (IPs que o cliente usará para se conectar ao acesso remoto) foram decididos como 131.107.0.102, 131.107.0.103  
  
2.  Balanceador de carga na rede externa Self-IPs-131.107.0.245 (Internet), 131.107.1.245  
  
    A rede de perímetro (também conhecida como zona desmilitarizada e DMZ) está entre o balanceador de carga na rede externa e o servidor de acesso remoto.  
  
3.  Endereços IP para o servidor de acesso remoto na rede de perímetro-131.107.1.102, 131.107.1.103  
  
4.  Endereços IP para o servidor de acesso remoto na rede ELB (ou seja, entre o servidor de acesso remoto e o balanceador de carga na rede interna)-30.11.1.101, 2006:2005:11:1::101  
  
5.  Balanceador de carga na rede interna Self-IPs-30.11.1.245 2006:2005:11:1::245 (ELB), 30.1.1.245 2006:2005:1:1::245 (corpnet)  
  
6.  Os VIPs internos (endereços IP usados para investigação da Web do cliente e para o servidor de local de rede, se instalados nos servidores de acesso remoto) foram decididos como 30.1.1.10, 2006:2005:1:1::10  
  
##### <a name="steps"></a>Etapas  
  
1.  Configure o adaptador de rede externo do servidor de acesso remoto (que está conectado à rede de perímetro) com os endereços 131.107.0.102, 131.107.0.103. Essa etapa é necessária para que a configuração do DirectAccess detecte os pontos de extremidade de túnel IPsec corretos.  
  
2.  Configure o adaptador de rede interno do servidor de acesso remoto (que está conectado à rede ELB) com os endereços IP do servidor de local de investigação/rede (30.1.1.10, 2006:2005:1:1::10). Essa etapa é necessária para permitir que os clientes acessem o IP da investigação da Web, de modo que o assistente de conectividade de rede indique corretamente o status da conexão com o DirectAccess. Essa etapa também permite o acesso ao servidor de local de rede, se estiver configurado no servidor DirectAccess.  
  
    > [!NOTE]  
    > Verifique se o controlador de domínio pode ser acessado do servidor de acesso remoto com essa configuração.  
  
3.  Configure o servidor único do DirectAccess no servidor de acesso remoto.  
  
4.  Habilite o balanceamento de carga externo na configuração do DirectAccess. Use 131.107.1.102 como o endereço IP dedicado externo (DIP) (131.107.1.103 será selecionado automaticamente), use 30.11.1.101, 2006:2005:11:1::101 como o DIPs interno.  
  
5.  Configure o VIP (IPs virtuais) externos no balanceador externo de carga com os endereços 131.107.0.102 e 131.107.0.103. Além disso, configure os VIPs internos no balanceador externo de carga com os endereços 30.1.1.10 e 2006:2005:1:1::10.  
  
6.  O servidor de acesso remoto agora será configurado com os endereços IP planejados e os endereços IP internos e externos para o cluster serão configurados de acordo com os endereços IP planejados.  
  
## <a name="32-plan-ip-https"></a><a name="bkmk_2_2_NLB"></a>IP do plano 3,2-HTTPS  
  
1.  **Requisitos de certificado**-durante a implantação do único servidor de acesso remoto selecionado para usar um certificado IP-HTTPS emitido por uma autoridade de certificação pública ou interna (CA) ou um certificado autoassinado. Para a implantação de cluster, você deve usar um tipo de certificado idêntico em cada membro do cluster de acesso remoto. Ou seja, se você usou um certificado emitido por uma AC pública (recomendado), deverá instalar um certificado emitido por uma autoridade de certificação pública em cada membro do cluster. O nome da entidade do novo certificado deve ser idêntico ao nome da entidade do certificado IP-HTTPS usado atualmente na sua implantação. Observe que, se você estiver usando certificados autoassinados, eles serão configurados automaticamente em cada servidor durante a implantação do cluster.  
  
2.  **Requisitos de prefixo**-o acesso remoto permite o balanceamento de carga do tráfego baseado em SSL e do tráfego do DirectAccess. Para balancear a carga de todo o tráfego do DirectAccess baseado em IPv6, o acesso remoto deve examinar o túnel IPv4 para todas as tecnologias de transição. Como o tráfego IP-HTTPS é criptografado, não é possível examinar o conteúdo do túnel IPv4. Para habilitar o balanceamento de carga do tráfego IP-HTTPS, você deve alocar um prefixo IPv6 grande o suficiente para que um prefixo IPv6/64 diferente possa ser atribuído a cada membro do cluster. Você pode configurar um máximo de 32 servidores em um cluster com balanceamento de carga; Portanto, você deve especificar um prefixo/59. Esse prefixo deve ser roteável para o endereço IPv6 interno do cluster de acesso remoto e é configurado no assistente de instalação do servidor de acesso remoto.  
  
    > [!NOTE]  
    > Os requisitos de prefixo são relevantes apenas em uma rede interna habilitada para IPv6 (somente IPv6 ou IPV4 + IPv6). Em uma rede corporativa somente IPv4, o prefixo do cliente é configurado automaticamente e o administrador não pode alterá-lo.  
  
## <a name="33-plan-for-vpn-client-connections"></a><a name="BKMK_3.3"></a>3,3 plano para conexões de cliente VPN  
Há várias considerações para conexões de cliente VPN:  
  
-   O tráfego do cliente VPN não poderá ser balanceado se os endereços do cliente VPN forem alocados usando DHCP. Um pool de endereços estáticos é necessário.  
  
-   O RRAS pode ser habilitado em um cluster com balanceamento de carga que foi implantado somente para DirectAccess, usando **habilitar VPN** no painel Tarefas do console de gerenciamento de acesso remoto.  
  
-   Todas as alterações de VPN concluídas no console de gerenciamento de roteamento e acesso remoto (rrasmgmt. msc) precisarão ser replicadas manualmente em todos os servidores de acesso remoto no cluster.  
  
-   Para habilitar o tráfego de cliente IPv6 VPN para balanceamento de carga, você deve especificar um prefixo de IPv6 de 59 bits.  
  
## <a name="34-plan-the-network-location-server"></a><a name="BKMK_nls"></a>3,4 planejar o servidor do local de rede  
Se você estiver executando o site do servidor de local de rede no único servidor de acesso remoto, durante a implantação, você optou por usar um certificado emitido por uma autoridade de certificação interna (CA) ou um certificado autoassinado.  Observe o seguinte:  
  
1.  Cada membro do cluster de acesso remoto deve ter um certificado para o servidor de local de rede que corresponde à entrada DNS para o site do servidor de local de rede.  
  
2.  O certificado para cada servidor de cluster deve ser emitido da mesma forma que o certificado no servidor de localização de rede atual certificado de servidor de local de redes. Por exemplo, se você usou um certificado emitido por uma AC interna, deverá instalar um certificado emitido pela autoridade de certificação interna em cada membro do cluster.  
  
3.  Se você usou um certificado autoassinado, um certificado autoassinado será configurado automaticamente para cada servidor durante a implantação do cluster.  
  
4.  O nome da entidade do certificado não deve ser idêntico ao nome de qualquer um dos servidores na implantação de acesso remoto.  
