---
title: BGP (Border Gateway Protocol)
description: Você pode usar este tópico para obter uma compreensão do Border Gateway Protocol (BGP) no Windows Server 2016, incluindo topologias de implantação de BGP tem suportada e recursos do BGP e recursos.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78cc2ce3-a48e-45db-b402-e480b493fab1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65af14e3adfacd96334e2326f8dd0b346e27034a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850177"
---
# <a name="border-gateway-protocol-bgp"></a>BGP (Border Gateway Protocol)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter uma compreensão do Border Gateway Protocol (BGP), incluindo topologias de implantação com suporte do BGP e recursos e capacidades do BGP.  
  
> [!NOTE]  
> Além deste tópico, a seguinte documentação de BGP está disponível.  
>   
> -   [Referência de comando do PowerShell do Windows via protocolo BGP](../../remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
Este tópico contém as seguintes seções.  
  
-   [Suporte a BGP topologias de implantação](#bkmk_top)  
  
-   [Recursos do BGP](#bkmk_features)  
  
Quando configuradas em um serviço de acesso remoto do Windows Server 2016 \(RAS\) Gateway no modo multilocatário, Border Gateway Protocol (BGP) fornece a capacidade de gerenciar o roteamento de tráfego de rede entre redes VM de seus locatários e seus sites remotos. Você também pode usar o BGP para implantações de Gateway RAS de locatário único e, quando você implanta o acesso remoto como uma rede Local \(LAN\) roteador.  
  
O BGP reduz a necessidade de configuração de roteamento manual em roteadores, porque ele é um protocolo de roteamento dinâmico e aprende rotas entre sites conectados usando conexões VPN site a site automaticamente.  
  
Para usar o roteamento de BGP, você deve instalar o **serviço de acesso remoto \(RAS\)**  e/ou o **roteamento** serviço de função da função de servidor de acesso remoto em um computador ou máquina virtual \(VM\) -o tipo de sistema que você usa depende se você tem uma implantação de multilocatário:  
  
-   Para uma implantação de multilocatária, é recomendável que você instale o Gateway de RAS em uma ou mais VMs. Uso de várias VMs fornece alta disponibilidade. O Gateway de RAS é capaz de manipular várias conexões de vários locatários e consiste em um host Hyper-V e uma VM que, na verdade, é configurada como o gateway. Esse gateway é configurado com conexões de VPN site a site como um roteador BGP multilocatário para o locatário do exchange e o provedor de serviços de nuvem \(CSP\) rotas de sub-rede.  
  
-   Para uma implantação de gateway de borda de locatário único ou uma implantação do roteador de LAN, você pode instalar o Gateway de RAS em um computador físico ou uma máquina virtual.  
  
> [!IMPORTANT]  
> Quando você instala um Gateway de RAS, você deve especificar se o BGP é habilitado para cada locatário usando o **Enable-RemoteAccessRoutingDomain** comando do Windows PowerShell com o **tipo** valor do parâmetro de  **Todos os**. Para instalar o acesso remoto como um roteador de LAN com BGP habilitado sem recursos multilocatário, você pode usar o comando **Install-RemoteAccess - VpnType RoutingOnly**.  
>   
> O exemplo de código a seguir ilustra como instalar o acesso remoto no modo de multilocação com todos os recursos de acesso remoto (VPN ponto a site, VPN site a site e BGP roteamento) habilitados para dois locatários, Contoso e Fabrikam.  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
## <a name="bkmk_top"></a>Suporte a BGP topologias de implantação  
As topologias de implantação com suporte em que sites corporativos são conectados a um datacenter do provedor de serviços de nuvem (CSP) estão listadas abaixo.  
  
Todos os cenários, o gateway CSP é um Gateway de RAS do Windows Server 2016 na borda. O Gateway de RAS, que é capaz de manipular várias conexões de vários locatários, consiste em um host Hyper-V e uma VM que, na verdade, é configurada como o gateway. Esse gateway de extremidade é configurado com conexões VPN site a site como um roteador BGP multilocatário para troca de rotas de sub-rede corporativa e de CSP.  
  
Locatários conectam seus recursos no datacenter do CSP usando uma conexão de VPN site a site (S2S). Além disso, o protocolo de roteamento de BGP é implantado para troca de informações de roteamento dinâmico entre os gateways Enterprise e o CSP.  
  
Há suporte para as seguintes topologias de implantação.  
  
-   [Gateway do RAS VPN Site a Site com BGP na extremidade do site corporativo](#bkmk_top1)  
  
-   [Gateway de terceiros com BGP na extremidade do site corporativo](#bkmk_top2)  
  
-   [Vários sites corporativos com gateways de terceiros](#bkmk_top3)  
  
-   [Pontos de encerramento separados para BGP e VPN](#bkmk_top4)  
  
As seções a seguir contêm informações adicionais sobre cada topologia BGP com suporte.  
  
### <a name="bkmk_top1"></a>Gateway do RAS VPN Site a Site com BGP na extremidade do site corporativo  
Esta topologia ilustra um site corporativo conectado a um CSP. A topologia de roteamento de Enterprise inclui um roteador interno, um Gateway de RAS do Windows Server 2016 é configurado para conexões VPN de site a site com o CSP e um dispositivo de firewall de borda. O Gateway de RAS encerra as conexões VPN S2S e BGP.  
  
![Gateway do acesso remoto VPN Site a Site](../../media/Border-Gateway-Protocol-BGP/bgp_01.jpg)  
  
Ambos os sites são conectados usando o External Border Gateway Protocol (eBGP), que pode transmitir informações entre os roteadores habilitados para BGP em sistemas autônomos separados (AS). Isso requer que a empresa e o CSP tenham números de sistema autônomo (ASN) diferentes, que é um parâmetro que é parte integrante do protocolo BGP.  
  
Nesse cenário, o BGP funciona da seguinte maneira.  
  
-   O dispositivo de extremidade de site corporativo aprende as rotas de sub-rede virtualizadas (10.2.1.0/24) hospedadas na nuvem usando o BGP. Este dispositivo também anuncia as rotas de sub-rede local (10.1.1.0/24) para o Gateway de multilocatário de RAS do CSP.  
  
-   O roteador de extremidade do cliente aprende rotas internas locais por meio de um dos seguintes mecanismos:  
  
    -   O dispositivo de extremidade executa o BGP com um roteador interno e aprende rotas internas (neste exemplo, 10.1.1.0/24). Enquanto isso, o roteador interno aprende rotas externas (como 10.2.1.0/24) do dispositivo de extremidade, e o roteador interno deve distribuir essas rotas para outros roteadores locais usando um Interior Gateway Protocol (IGP), como Open Shortest Path First (OSPF) ou Routing Information Protocol (RIP).  
  
    -   O dispositivo de borda extremidade ser configurado com rotas estáticas ou interfaces para selecionar as rotas para anúncio usando o BGP. O dispositivo de extremidade também distribui as rotas externas para outros roteadores locais usando um IGP.  
  
### <a name="bkmk_top2"></a>Gateway de terceiros com BGP na extremidade do site corporativo  
Essa topologia ilustra um site corporativo usando um roteador de extremidade de terceiros para se conectar a um CSP. O roteador de extremidade também serve como um gateway VPN site a site.  
  
![Gateway de terceiros com BGP na extremidade do site corporativo](../../media/Border-Gateway-Protocol-BGP/bgp_02.jpg)  
  
O roteador de extremidade corporativo aprende rotas internas locais por meio de um dos seguintes mecanismos:  
  
-   O dispositivo de extremidade executa o BGP com um roteador interno e aprende rotas internas (neste exemplo, 10.1.1.0/24).  
  
-   O dispositivo de extremidade implementa um Interior Gateway Protocol (IGP) e participa diretamente do roteamento interno.  
  
### <a name="bkmk_top3"></a>Datacenter de nuvem de vários sites corporativos, conectar-se para o CSP  
Esta topologia ilustra vários sites corporativos que usam gateways de terceiros para se conectar a um CSP. Os dispositivos de extremidade de terceiros servem como gateways de VPN site a site e roteadores BGP.  
  
![Datacenter de nuvem de vários sites corporativos, conectar-se para o CSP](../../media/Border-Gateway-Protocol-BGP/bgp_03.jpg)  
  
O roteador de extremidade do cliente aprende rotas internas locais por meio de um dos seguintes mecanismos:  
  
-   O dispositivo de extremidade executa o BGP com um roteador interno e aprende rotas internas (neste exemplo, 10.1.1.0/24).  
  
-   O dispositivo de extremidade implementa um Interior Gateway Protocol (IGP) e participa diretamente do roteamento interno.  
  
Cada site corporativo aprende as rotas do outro site sobre a conectividade direta eBGP.  
  
Cada site corporativo descobre as rotas de rede hospedada diretamente e usando o outro site corporativo, mas seleciona a melhor rota com base no custo da rota.  
  
Se o roteador BGP no Site corporativo 1 não pode se conectar com o roteador BGP de 2 de Site de empresa porque conectividade tiver falhado, o roteador BGP do Site 1 começa dinamicamente a aprender as rotas à rede da empresa Site 2 com o roteador BGP do CSP e o tráfego é perfeitamente redirecionado do Site 1 para o Site 2 por meio do roteador de BGP do Windows Server no CSP.  
  
### <a name="bkmk_top4"></a>Pontos de encerramento separados para BGP e VPN  
Esta topologia ilustra uma empresa que usa dois roteadores diferentes, como o BGP e pontos de extremidade VPN site a site. VPN site a site é encerrada no Gateway de RAS do Windows Server 2016, enquanto o BGP é encerrado em um roteador interno. No lado do CSP das conexões, o CSP encerra as conexões VPN e BGP com o Gateway de RAS. Com essa configuração, o hardware do roteador de terceiros interno deve oferecer suporte à redistribuição de rotas IGP para BGP, bem como redistribuir as rotas BGP para IGP.  
  
![Pontos de encerramento separados para BGP e VPN](../../media/Border-Gateway-Protocol-BGP/bgp_04.jpg)  
  
O roteador interno aprende as rotas corporativas por meio de um dos seguintes mecanismos:  
  
-   BGP  
  
-   Um Interior Gateway Protocol (IGP) como OSPF ou RIP.  
  
-   Configuração de roteamento estático  
  
Quando qualquer IGP é usado no site da empresa, o roteador interno deve redistribuir rotas de IGP no BGP, bem como redistribuir rotas de BGP para rotas de IGP, para manter a conectividade de sub-rede entre redes virtuais CSP e sub-redes locais corporativas.  
  
Com essa implantação, o Gateway de RAS Enterprise tem uma conexão de VPN site a site com o Gateway de RAS do CSP, que fornece o Gateway de RAS corporativo com as rotas para o gateway CSP. O roteador corporativo interno, em seguida, aprende essa rota para o gateway CSP usando o iBGP com o Gateway de RAS corporativo. Por isso, o roteador corporativo interno, em seguida, é capaz de estabelecer uma sessão de emparelhamento com o roteador BGP de Gateway de RAS de CSP.  
  
Desse ponto, o roteador corporativo interno e o Gateway de RAS do CSP trocam informações de roteamento. E o roteador BGP de RAS corporativo aprende rotas de CSP e rotas corporativas para rotear fisicamente pacotes entre as redes.  
  
## <a name="bkmk_features"></a>Recursos do BGP  
A seguir estão os recursos do roteador de BGP do Gateway RAS.  
  
**Roteamento de BGP como um serviço de função de acesso remoto**. Agora você pode instalar o **roteamento** serviço de função da função de servidor de acesso remoto sem instalar o **serviço de acesso remoto (RAS)** serviço de função quando quiser usar o acesso remoto como um roteador BGP LAN.  Isso reduz o volume de memória do roteador BGP e instala apenas os componentes necessários para roteamento dinâmico do BGP. O serviço de função de roteamento é útil quando apenas uma VM do roteador BGP é necessária, e não exigem o uso do DirectAccess ou VPN. Além disso, usando o acesso remoto como um roteador de LAN com BGP fornece as vantagens de roteamentos dinâmicas do BGP em sua rede interna.  
  
**Estatísticas de BGP (contadores de mensagens, contadores de rota)**. O roteador BGP dá suporte à exibição das mensagens e estatísticas de rota, se necessário, usando o comando **Get-BgpStatistics** do Windows PowerShell.  
  
**Suporte a Equal Cost Multi Path Routing (ECMP)**. O roteador BGP dá suporte à ECMP e pode ter mais de uma rota de custo igual inseridas na tabela e na pilha de roteamento do BGP. A seleção de roteador BGP da rota para a transmissão de pacotes de dados é aleatória com ECMP habilitado.  
  
**Configuração de HoldTime**. O roteador BGP dá suporte à configuração do valor HoldTimer de acordo com seus requisitos de rede. O temporizador pode ser alterado dinamicamente para acomodar a interoperabilidade com dispositivos de terceiros ou para manter um horário máximo específico de tempo limite da sessão de emparelhamento via protocolo BGP.  
  
**Suporte a BGP interno e externo**. O roteador BGP dá suporte ao emparelhamento de iBGP e eBGP. Para configurar qualquer um deles, você deve garantir que o ASNs apropriados são atribuídos aos roteadores BGP local e remoto. Todas as quatro topologias de implantação de BGP empregam o uso do emparelhamento de eBGP, e a quarta topologia usa também o emparelhamento de iBGP.  
  
**Interoperabilidade com soluções de terceiros**. O roteador BGP baseia-se na especificação mais recente do BGP versão 4 e foi testado quanto à interoperabilidade com a maioria dos principais dispositivos de roteamento BGP de terceiros. Para obter mais informações, consulte Request for Comments (RFC) 4271, [Um Border Gateway Protocol 4 (BGP-4)](https://tools.ietf.org/html/rfc4271).  
  
**Suporte a emparelhamento de transporte de IPv4 e IPv6**. O roteador BGP dá suporte ao emparelhamento de IPv4 e IPv6. No entanto, você deve configurar o identificador de BGP como o endereço IPv4 do roteador BGP. Para todas as topologias de implantação do roteador BGP, qualquer um dos dois tipos de emparelhamento (IPV4 / IPv6) pode ser usado.  
  
**Funcionalidade de aprendizagem e anúncio do roteamento unicast de IPv4 e IPv6 (Multiprotocol Network Layer Reachability Information [NLRI])**. Não importa qual transporte você use, o roteador BGP pode trocar rotas IPv4 e IPv6 se a funcionalidade apropriada for anunciado por outros roteadores BGP durante o estabelecimento da sessão. Para configurar o roteamento IPv6, o parâmetro IPv6Routing deve estar habilitado e um endereço IPv6 Global Local deve ser configurado no nível do roteador.  
  
**Emparelhamento de modo misto e de modo passivo**. Você pode configurar sessões de emparelhamento via protocolo BGP no modo misto - onde o roteador BGP atua como iniciador e Respondente, ou no modo passivo, em que o roteador BGP não inicia o emparelhamento, mas responder às solicitações de entrada. O modo misto é o padrão e é recomendado para emparelhamento via protocolo BGP. Isso se aplica, a menos que você queira usar o modo passivo para fins de depuração e diagnóstico. Para todas as topologias de implantação do roteador BGP, o emparelhamento de modo misto é necessário para permitir reinícios automáticos no caso de eventos de falha.  
  
**Funcionalidade e reconfiguração de atributo da rota**. Você pode adicionar, modificar ou remover os seguintes atributos dos anúncios de rota de entrada e saída do roteador BGP usando as políticas de roteamento BGP Next-Hop, MED, Local-Pref e Community.  
  
**Filtragem de rotas**. O roteador BGP dá suporte à filtragem de anúncios de rota de entrada ou de saída com base em vários atributos de rota como prefixo, intervalo ASN, comunidade e do próximo salto.  
  
**Cliente de refletor de rota (RR) e RR**. O roteador BGP pode agir como um refletor de rota e um cliente de RR. Isso é útil em topologias complexas, onde RR pode simplificar a rede, formando RR Clusters.  
  
**Suporte de atualização de rota**. O roteador BGP dá suporte à atualização de rota e anuncia essa funcionalidade no emparelhamento por padrão. Ele é capaz de enviar um novo conjunto de atualizações de rota quando solicitado por um colega por meio de mensagem de atualização de rota, bem como o envio de uma atualização de rota para atualizar sua tabela de roteamento de eventos como alterações de política de roteamento para um par. Isso permite que o cenário de alterar ou atualizar as políticas de roteamento de BGP no Windows Server 2016 sem a necessidade de reiniciar o emparelhamento.  
  
**Suporte à configuração de roteamento estático**. Você pode configurar rotas estáticas ou interfaces no roteador BGP usando o comando do Windows PowerShell **Add-BgpCustomRoute**. As rotas estáticas que você configurar podem ser os prefixos ou o nome das interfaces por meio dos quais as rotas devem ser escolhidas. No entanto, apenas as rotas com saltos seguintes resolvíveis são inseridas nas tabelas de roteamento de BGP e anunciadas para colegas.  
  
**Suporte a roteamento de tráfego**. O roteador BGP dá suporte a roteamento de trânsito para iBGP para conexões iBGP, iBGP para eBGP e eBGP para eBGP.  
  
**Rotear bater retardamento**. Bater retardamento de rota para o roteamento de BGP no Windows Server 2016 fornece suporte de retardamento de bater de rota. Por exemplo, quando uma rota é constantemente sendo anunciada e retirado, tornando a tabela de roteamento instável, você pode configurar o roteador BGP para atribuir um peso de retardamento para a rota e monitorá-lo para flaps - e suprimir adequadamente ou un-suprimi-lo conforme necessário. Isso ajuda a manter uma tabela de roteamento estável e com menos processamento pelo roteador BGP.  
  
**Agregação de rota**. Agregação de rota para o roteador BGP fornece a capacidade de configurar rotas agregadas e substituir os anúncios de rota mais granulares com rotas de resumidas ou de agregação para pares. Isso resulta em um número menor número de mensagens de anúncio de rota transmitido pela rede.  
  
> [!NOTE]  
> No System Center, o Gateway de RAS é chamado de Gateway do Windows Server.  
  

  

