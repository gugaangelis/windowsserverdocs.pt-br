---
title: Arquitetura de implantação do Gateway de RAS
description: Você pode usar este tópico para saber mais sobre a implantação de provedor de serviços de nuvem (CSP) do Gateway de RAS no Windows Server 2016, incluindo os pools de Gateway de RAS, Refletores de rota e a implantação de vários gateways para locatários individuais.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3895e25a2af0437deb9eebe4ad89b110cfc9f2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870267"
---
# <a name="ras-gateway-deployment-architecture"></a>Arquitetura de implantação do Gateway de RAS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre a implantação de provedor de serviços de nuvem (CSP) do Gateway de RAS, incluindo os pools de Gateway de RAS, Refletores de rota e a implantação de vários gateways para locatários individuais.  
  
As seções a seguir fornecem visões gerais breves de alguns dos novos recursos do Gateway de RAS para que você possa entender como usar esses recursos no design da implantação do gateway.  
  
Além disso, um exemplo de implantação é fornecido, incluindo informações sobre o processo de adição de novos locatários, sincronização de rota e roteamento de plano de dados, gateway e failover de refletor de rota e muito mais.  
  
Este tópico contém as seguintes seções.  
  
-   [Usando novos recursos do Gateway RAS para criar a sua implantação](#bkmk_new)  
  
-   [Exemplo de implantação](#bkmk_example)  
  
-   [Adição de novos locatários e o espaço de endereço (CA) de cliente EBGP emparelhamento](#bkmk_tenant)  
  
-   [Sincronização de rota e os dados do plano de roteamento](#bkmk_route)  
  
-   [Como o controlador de rede responde ao Gateway RAS e Failover de refletor de rota](#bkmk_failover)  
  
-   [Vantagens de usar os novos recursos de Gateway RAS](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>Usando novos recursos do Gateway RAS para criar a sua implantação  
Gateway de RAS inclui vários novos recursos que são alterados e melhorar a maneira em que você implanta sua infraestrutura de gateway em seu datacenter.  
  
### <a name="bgp-route-reflector"></a>Refletor de rota BGP  
A capacidade de refletor de rota do Border Gateway Protocol (BGP) agora está incluído com o Gateway de RAS e fornece uma alternativa para a topologia de malha completa de BGP que normalmente é necessária para sincronização de rota entre roteadores. Com a sincronização de malha completa, todos os roteadores BGP devem se conectar com todos os outros roteadores na topologia de roteamento. No entanto, quando você usa o refletor de rota, o refletor de rota é o único roteador que se conecta com todos os outros roteadores, chamados clientes refletor de rota BGP, simplificando assim o roteiro de sincronização e reduzindo o tráfego de rede. O refletor de rota aprende todas as rotas, calcula melhores rotas e redistribui as rotas melhor para seus clientes BGP.  
  
Para obter mais informações, consulte [o que há de novo no Gateway de RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="bkmk_pools"></a>Pools de gateway  
No Windows Server 2016, você pode criar vários pools de gateway de tipos diferentes. Pools de gateway contêm muitas instâncias do Gateway de RAS e rotear o tráfego de rede entre redes físicas e virtuais.  
  
Para obter mais informações, consulte [o que há de novo no Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [alta disponibilidade do Gateway de RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_gps"></a>Redimensionamento do Pool de gateway  
Você pode facilmente dimensionar um pool de gateway para cima ou para baixo, adicionando ou removendo as VMs de gateway no pool. Remoção ou adição de gateways não interrompa os serviços que são fornecidos por um pool. Você também pode adicionar e remover pools inteiros de gateways.  
  
Para obter mais informações, consulte [o que há de novo no Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [alta disponibilidade do Gateway de RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_m"></a>Redundância do Pool de Gateway de M + N  
Cada pool de gateway é com redundância M + N. Isso significa que um estou ' número de VMs de gateway Active Directory seja realizado por um número de ' n' de VMs de gateway em espera. M + N redundância fornece mais flexibilidade na determinação do nível de confiabilidade que você precisa, quando você implanta o Gateway de RAS.  
  
Para obter mais informações, consulte [o que há de novo no Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [alta disponibilidade do Gateway de RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_example"></a>Exemplo de implantação  
A ilustração a seguir fornece um exemplo com eBGP emparelhamento em conexões de VPN site a site configuradas entre dois locatários, Contoso e Woodgrove e datacenter do CSP da Fabrikam.  
  
![eBGP emparelhamento sobre VPN site a site](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
Neste exemplo, Contoso requer largura de banda de gateway adicionais, à esquerda para a decisão de design de infraestrutura de gateway para encerrar o site Contoso Los Angeles no GW3 em vez de GW2. Por isso, as conexões de VPN de Contoso em sites diferentes encerrar no datacenter do CSP em dois gateways diferentes.  
  
Ambos esses gateways, GW2 e GW3, eram os Gateways de RAS primeiro configurado pelo controlador de rede quando o CSP adicionado aos locatários Contoso e Woodgrove à sua infra-estrutura. Por isso, esses dois gateways são configurados como Refletores de rota para esses clientes correspondentes (ou locatários). GW2 é o refletor de rota da Contoso e GW3 é o refletor de rota do Woodgrove - além de ser o ponto de encerramento de Gateway de RAS do CSP para a conexão VPN com o site Contoso HQ de Los Angeles.  
  
> [!NOTE]  
> Um Gateway de RAS pode rotear o tráfego de rede física e virtual para locatários diferentes até cem, dependendo dos requisitos de largura de banda de cada locatário.  
  
Como Refletores de rota, GW2 envia rotas de espaço de CA da Contoso para o controlador de rede e GW3 envia as rotas de espaço de CA do Woodgrove para controlador de rede.  
  
Controlador de rede envia por push as políticas de virtualização de rede do Hyper-V para a Contoso e Woodgrove redes virtuais, bem como as políticas de acesso remoto aos Gateways de RAS e políticas para multiplexadores (MUXes) que são configurados como um Software de balanceamento de carga de balanceamento de carga pool.  
  
## <a name="bkmk_tenant"></a>Adicionando novos locatários e o espaço de endereço de cliente (CA) eBGP emparelhamento  
Quando você assinar um novo cliente e adicionar o cliente como um novo locatário em seu datacenter, você pode usar o processo a seguir, muitos dos quais é executado automaticamente pelo controlador de rede e Gateway de RAS roteadores de eBGP.  
  
1.  Provisione uma nova rede virtual e cargas de trabalho de acordo com os requisitos do seu locatário.  
  
2.  Se necessário, configure a conectividade remota entre o site de empresa do locatário remoto e sua rede virtual em seu datacenter. Quando você implanta uma conexão de VPN site a site para o locatário, o controlador de rede automaticamente seleciona uma VM de Gateway de RAS disponível do pool de gateway disponíveis e configura a conexão.  
  
3.  Ao configurar a VM de Gateway de RAS para o novo locatário, o controlador de rede também configura o Gateway de RAS como um roteador BGP e designa como o refletor de rota para o locatário. Isso é verdadeiro mesmo em circunstâncias em que o Gateway de RAS serve como um gateway ou como um gateway e o refletor de rota para outros locatários.  
  
4.  Dependendo se o roteamento de espaço com a autoridade de certificação estiver configurado para redes de uso configurado estaticamente ou roteamento de BGP dinâmico, o controlador de rede configura as rotas estáticas correspondentes, vizinhos de BGP ou ambos na VM de Gateway de RAS e refletor de rota.  
  
    > [!NOTE]  
    > -   Depois que o controlador de rede tiver configurado um Gateway de RAS e refletor de rota para o locatário, sempre que o mesmo locatário requer uma nova conexão de VPN site a site, o controlador de rede verifica a capacidade disponível nessa VM de Gateway de RAS. Se o gateway original pode atender a capacidade necessária, a nova conexão de rede também é configurado na mesma VM de Gateway de RAS. Se a VM de Gateway de RAS não pode lidar com a capacidade adicional, o controlador de rede seleciona uma nova VM de Gateway de RAS disponíveis e configura a nova conexão nele. Essa nova VM de Gateway de RAS associado ao locatário torna-se o cliente de refletor de rota do locatário original refletor de rota do Gateway de RAS.  
    > -   Porque o pool de Gateway de RAS está por trás de balanceadores de carga de Software (SLBs), endereços VPN site a site dos locatários cada um usar um único endereço IP público, chamado de um endereço IP virtual (VIP), que é traduzido pelos SLBs para um endereço IP interno de datacenter, chamado um endereço IP dinâmico (DIP) para um Gateway de RAS que roteia o tráfego para o locatário da empresa. Esse mapeamento de endereço IP pública para privada pelo SLB garante que os túneis VPN site a site corretamente são estabelecidos entre os sites corporativos e os Gateways de RAS do CSP e Refletores de rota.  
    >   
    >     Para obter mais informações sobre o SLB, VIPs e quedas, consulte [balanceamento de carga de Software &#40;SLB&#41; para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Após o túnel VPN site a site entre o site do Enterprise e datacenter do CSP, que gateway de RAS é estabelecida para o novo locatário, as rotas estáticas que estão associadas com os túneis são automaticamente provisionadas nos lados da empresa e o CSP do túnel .  
  
6.  Com o espaço do CA BGP eBGP emparelhamento entre os sites corporativos e o refletor de rota de Gateway de RAS do CSP do roteamento, também é estabelecida.  
  
## <a name="bkmk_route"></a>Sincronização de rota e os dados do plano de roteamento  
Depois que o emparelhamento de eBGP é estabelecido entre sites corporativos e o refletor de rota de Gateway de RAS do CSP, o refletor de rota aprende todas as rotas da empresa usando o roteamento de BGP dinâmico. O refletor de rota sincroniza essas rotas entre todos os clientes de refletor de rota para que eles sejam configurados com o mesmo conjunto de rotas.  
  
Refletor de rota também atualiza essas rotas consolidadas, usando a sincronização de rota, ao controlador de rede. Controlador de rede, em seguida, converte as rotas para as políticas de virtualização de rede do Hyper-V e configura a rede de malha para garantir que o roteamento do caminho de dados de ponta a ponta seja provisionado. Este processo, o locatário fica acessível do locatário empresarial de rede virtual sites.  
  
Roteamento de plano de dados, os pacotes que acessar as VMs de Gateway de RAS são roteados diretamente à rede de virtual do locatário, porque as rotas necessárias agora estão disponíveis com todas as VMs de Gateway de RAS participantes.  
  
Da mesma forma, com as políticas de virtualização de rede do Hyper-V no local, rede virtual do locatário roteia os pacotes diretamente para as VMs de Gateway de RAS (sem a necessidade de saber sobre o refletor de rota) e, em seguida, os sites empresariais nos túneis VPN site a site .  
  
Além disso. tráfego de retorno da rede virtual do locatário para o site de empresa do locatário remoto ignora SLBs, um processo chamado de retorno de servidor direto (DSR).  
  
## <a name="bkmk_failover"></a>Como o controlador de rede responde ao Gateway RAS e Failover de refletor de rota  
A seguir estão dois cenários possíveis de failover - um para clientes de refletor de rota do Gateway de RAS - e outro para Refletores de rota do Gateway de RAS incluindo informações sobre como o controlador de rede trata failover para VMs na configuração.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Falha VM de um cliente de refletor de rota BGP de Gateway RAS  
Controlador de rede realiza as seguintes ações quando um cliente de refletor de rota do Gateway de RAS falhar.  
  
> [!NOTE]  
> Quando um Gateway de RAS não é um refletor de rota para a infraestrutura BGP de um locatário, ele é um cliente de refletor de rota na infraestrutura BGP do locatário.  
  
-   Controlador de rede seleciona uma VM de Gateway de RAS em espera disponível e provisiona a nova VM de Gateway de RAS com a configuração da VM de Gateway de RAS com falha.  
  
-   Controlador de rede atualiza a configuração do SLB correspondente para garantir que os túneis de VPN site a site de sites de locatário para o Gateway de RAS com falha são estabelecidos corretamente com o novo Gateway de RAS.  
  
-   Controlador de rede configura o cliente de refletor de rota BGP no gateway de novo.  
  
-   Controlador de rede configura o novo cliente de refletor de rota de BGP de Gateway de RAS como ativa. O Gateway de RAS inicia imediatamente o emparelhamento com refletor de rota do locatário para compartilhar informações de roteamento e para habilitar o emparelhamento de eBGP para o site de empresa correspondente.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Falha VM para um refletor de rota BGP do Gateway RAS  
Controlador de rede realiza as seguintes ações quando um refletor de rota de BGP de Gateway de RAS falhar.  
  
-   Controlador de rede seleciona uma VM de Gateway de RAS em espera disponível e provisiona a nova VM de Gateway de RAS com a configuração da VM de Gateway de RAS com falha.  
  
-   Controlador de rede configura o refletor de rota na nova VM de Gateway de RAS e atribui a nova VM o mesmo endereço IP que foi usado pela VM com falha, fornecendo assim a integridade de rota, apesar da falha da VM.  
  
-   Controlador de rede atualiza a configuração do SLB correspondente para garantir que os túneis de VPN site a site de sites de locatário para o Gateway de RAS com falha são estabelecidos corretamente com o novo Gateway de RAS.  
  
-   Controlador de rede configura a nova VM RAS Gateway BGP rota Reflector como ativa.  
  
-   O refletor de rota fica imediatamente ativo. O túnel VPN site a site para a empresa é estabelecido, e o refletor de rota usa o emparelhamento de eBGP e troca rotas com os roteadores de site de empresa.  
  
-   Após a seleção de rotas BGP, as atualizações de refletor de rota de BGP de Gateway de RAS clientes refletor de rota no data center de locatário e sincroniza as rotas com o controlador de rede, disponibilizando o caminho de dados de ponta a ponta para o tráfego de locatário.  
  
## <a name="bkmk_advantages"></a>Vantagens de usar os novos recursos de Gateway RAS  
A seguir é algumas das vantagens de usar esses novos recursos de Gateway de RAS ao projetar sua implantação do Gateway de RAS.  
  
**Escalabilidade do Gateway de RAS**  
  
Porque você pode adicionar quantas VMs de Gateway de RAS conforme necessário para os pools de Gateway de RAS, você pode facilmente dimensionar sua implantação do Gateway de RAS para otimizar o desempenho e capacidade. Quando você adiciona VMs a um pool, você pode configurar esses Gateways de RAS com conexões de VPN site a site de qualquer tipo (IKEv2, L3, GRE), eliminar gargalos de capacidade sem nenhum tempo de inatividade.  
  
**Gerenciamento de Gateway do Site corporativo simplificado**  
  
Quando o locatário tiver vários sites corporativos, o locatário pode configurar todos os sites com um endereço de IP de VPN site a site remoto e um endereço IP do vizinho remoto único - seu datacenter do CSP RAS Gateway BGP VIP de refletor de rota para esse locatário. Isso simplifica o gerenciamento de gateway para seus locatários.  
  
**Correção rápida de falha do Gateway**  
  
Para garantir uma resposta de failover rápido, você pode configurar o tempo de parâmetro de Keepalive BGP entre as rotas de borda e o roteador de controle para um intervalo de hora abreviada, como menor que ou igual a dez segundos. Com esse intervalo de curto keep alive, se um roteador de borda de BGP do Gateway de RAS falhar, a falha é detectada rapidamente e controlador de rede segue as etapas fornecidas nas seções anteriores. Essa vantagem pode reduzir a necessidade de um protocolo de detecção de falha separados, como protocolo de detecção de encaminhamento bidirecional (BFD).  
  
 
  


