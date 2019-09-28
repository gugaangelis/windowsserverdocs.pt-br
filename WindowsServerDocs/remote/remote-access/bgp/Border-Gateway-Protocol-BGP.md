---
title: BGP (Border Gateway Protocol)
description: Você pode usar este tópico para obter uma compreensão do Border Gateway Protocol (BGP) no Windows Server 2016, incluindo topologias de implantação com suporte do BGP e recursos e funcionalidades do BGP.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78cc2ce3-a48e-45db-b402-e480b493fab1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ae6fddce1564e44ad72a5630c6abb16cdb6735d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388985"
---
# <a name="border-gateway-protocol-bgp"></a>BGP (Border Gateway Protocol)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para obter uma compreensão do Border Gateway Protocol (BGP), incluindo topologias de implantação com suporte do BGP e recursos e capacidades do BGP.  
  
> [!NOTE]  
> Além deste tópico, a seguinte documentação do BGP está disponível.  
>   
> -   [Referência de comando do Windows PowerShell BGP](../../remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
Este tópico contém as seguintes seções.  
  
-   [Topologias de implantação com suporte por BGP](#bkmk_top)  
  
-   [Recursos do BGP](#bkmk_features)  
  
Quando configurado em um serviço de acesso remoto do Windows Server 2016 \(RAS @ no__t-1 Gateway no modo multilocatário, o Border Gateway Protocol (BGP) fornece a capacidade de gerenciar o roteamento de tráfego de rede entre as redes de VM de seus locatários e suas remotas sites. Você também pode usar o BGP para implantações de gateway de RAS de locatário único e ao implantar o acesso remoto como uma rede local \(LAN @ no__t-1 router.  
  
O BGP reduz a necessidade de configuração de roteamento manual em roteadores, porque ele é um protocolo de roteamento dinâmico e aprende rotas entre sites conectados usando conexões VPN site a site automaticamente.  
  
Para usar o roteamento BGP, você deve instalar o **serviço de acesso remoto \(RAS @ no__t-2** e/ou o serviço de função de **Roteamento** da função de servidor acesso remoto em um computador ou máquina virtual \(VM @ no__t-5-o tipo de sistema usado depende Se você tem ou não uma implantação multilocatário:  
  
-   Para uma implantação multilocatário, é recomendável que você instale o gateway RAS em uma ou mais VMs. O uso de várias VMs fornece alta disponibilidade. O gateway RAS é capaz de lidar com várias conexões de vários locatários e consiste em um host Hyper-V e uma VM que é realmente configurada como o gateway. Esse gateway é configurado com conexões VPN site a site como um roteador BGP multilocatário para trocar o locatário e o provedor de serviços de nuvem \(CSP @ no__t-1 rotas de sub-rede.  
  
-   Para uma implantação de gateway de borda de locatário único ou uma implantação de roteador de LAN, você pode instalar o gateway de RAS em um computador físico ou em uma VM.  
  
> [!IMPORTANT]  
> Ao instalar um gateway de RAS, você deve especificar se o BGP está habilitado para cada locatário usando o comando **Enable-RemoteAccessRoutingDomain** do Windows PowerShell com o valor de parâmetro de **tipo** **All**. Para instalar o acesso remoto como um roteador de LAN habilitado para BGP sem recursos multilocatários, você pode usar o comando **install-RemoteAccess-VpnType RoutingOnly**.  
>   
> O código de exemplo a seguir ilustra como instalar o RAS no modo multilocação com todos os recursos de RAS (VPN ponto a site, VPN site a site e roteamento BGP) habilitados para dois locatários, contoso e fabrikam.  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
## <a name="bkmk_top"></a>Topologias de implantação com suporte por BGP  
As topologias de implantação com suporte em que sites corporativos são conectados a um datacenter do provedor de serviços de nuvem (CSP) estão listadas abaixo.  
  
Em todos os cenários, o gateway do CSP é um gateway de RAS do Windows Server 2016 na borda. O gateway de RAS, que é capaz de lidar com várias conexões de vários locatários, consiste em um host Hyper-V e uma VM que é realmente configurada como o gateway. Esse gateway de extremidade é configurado com conexões VPN site a site como um roteador BGP multilocatário para troca de rotas de sub-rede corporativa e de CSP.  
  
Locatários conectam seus recursos no datacenter do CSP usando uma conexão de VPN site a site (S2S). Além disso, o protocolo de roteamento de BGP é implantado para troca de informações de roteamento dinâmico entre os gateways Enterprise e o CSP.  
  
Há suporte para as seguintes topologias de implantação.  
  
-   [Gateway de site a site de VPN RAS com BGP no Enterprise site Edge](#bkmk_top1)  
  
-   [Gateway de terceiros com BGP no Enterprise site Edge](#bkmk_top2)  
  
-   [Vários sites corporativos com gateways de terceiros](#bkmk_top3)  
  
-   [Pontos de encerramento separados para BGP e VPN](#bkmk_top4)  
  
As seções a seguir contêm informações adicionais sobre cada topologia BGP com suporte.  
  
### <a name="bkmk_top1"></a>Gateway de site a site de VPN RAS com BGP no Enterprise site Edge  
Esta topologia ilustra um site corporativo conectado a um CSP. A topologia de roteamento corporativo inclui um roteador interno, um gateway de RAS do Windows Server 2016 configurado para conexões VPN site a site com o CSP e um dispositivo de firewall de borda. O gateway RAS encerra as conexões VPN e BGP S2S.  
  
![Gateway de site a site VPN RAS](../../media/Border-Gateway-Protocol-BGP/bgp_01.jpg)  
  
Ambos os sites são conectados usando o External Border Gateway Protocol (eBGP), que pode transmitir informações entre os roteadores habilitados para BGP em sistemas autônomos separados (AS). Isso requer que a empresa e o CSP tenham números de sistema autônomo (ASN) diferentes, que é um parâmetro que é parte integrante do protocolo BGP.  
  
Nesse cenário, o BGP funciona da seguinte maneira.  
  
-   O dispositivo de extremidade de site corporativo aprende as rotas de sub-rede virtualizadas (10.2.1.0/24) hospedadas na nuvem usando o BGP. Esse dispositivo também anuncia as rotas de sub-rede locais (10.1.1.0/24) para o gateway de multilocatário do CSP de RAS.  
  
-   O roteador de extremidade do cliente aprende rotas internas locais por meio de um dos seguintes mecanismos:  
  
    -   O dispositivo de extremidade executa o BGP com um roteador interno e aprende rotas internas (neste exemplo, 10.1.1.0/24). Enquanto isso, o roteador interno aprende rotas externas (como 10.2.1.0/24) do dispositivo de extremidade, e o roteador interno deve distribuir essas rotas para outros roteadores locais usando um Interior Gateway Protocol (IGP), como Open Shortest Path First (OSPF) ou Routing Information Protocol (RIP).  
  
    -   O dispositivo de borda extremidade ser configurado com rotas estáticas ou interfaces para selecionar as rotas para anúncio usando o BGP. O dispositivo de extremidade também distribui as rotas externas para outros roteadores locais usando um IGP.  
  
### <a name="bkmk_top2"></a>Gateway de terceiros com BGP no Enterprise site Edge  
Essa topologia ilustra um site corporativo usando um roteador de extremidade de terceiros para se conectar a um CSP. O roteador de extremidade também serve como um gateway VPN site a site.  
  
![Gateway de terceiros com BGP na extremidade do site corporativo](../../media/Border-Gateway-Protocol-BGP/bgp_02.jpg)  
  
O roteador de extremidade corporativo aprende rotas internas locais por meio de um dos seguintes mecanismos:  
  
-   O dispositivo de extremidade executa o BGP com um roteador interno e aprende rotas internas (neste exemplo, 10.1.1.0/24).  
  
-   O dispositivo de extremidade implementa um Interior Gateway Protocol (IGP) e participa diretamente do roteamento interno.  
  
### <a name="bkmk_top3"></a>Vários sites corporativos que se conectam ao datacenter de nuvem do CSP  
Esta topologia ilustra vários sites corporativos que usam gateways de terceiros para se conectar a um CSP. Os dispositivos de extremidade de terceiros servem como gateways de VPN site a site e roteadores BGP.  
  
![Vários sites corporativos que se conectam ao datacenter de nuvem do CSP](../../media/Border-Gateway-Protocol-BGP/bgp_03.jpg)  
  
O roteador de extremidade do cliente aprende rotas internas locais por meio de um dos seguintes mecanismos:  
  
-   O dispositivo de extremidade executa o BGP com um roteador interno e aprende rotas internas (neste exemplo, 10.1.1.0/24).  
  
-   O dispositivo de extremidade implementa um Interior Gateway Protocol (IGP) e participa diretamente do roteamento interno.  
  
Cada site corporativo aprende as rotas do outro site sobre a conectividade direta eBGP.  
  
Cada site corporativo descobre as rotas de rede hospedada diretamente e usando o outro site corporativo, mas seleciona a melhor rota com base no custo da rota.  
  
Se o roteador BGP no site da empresa 1 não puder se conectar ao roteador de BGP do site 2, pois a conectividade falhou, o roteador BGP do site 1 começa a aprender as rotas para a rede do site da empresa 2 a partir do roteador BGP do CSP e o tráfego está diretamente Redirecionado do site 1 para o site 2 por meio do roteador BGP do Windows Server no CSP.  
  
### <a name="bkmk_top4"></a>Pontos de encerramento separados para BGP e VPN  
Esta topologia ilustra uma empresa que usa dois roteadores diferentes, como o BGP e pontos de extremidade VPN site a site. A VPN site a site é encerrada no gateway RAS do Windows Server 2016, enquanto o BGP é encerrado em um roteador interno. No lado do CSP das conexões, o CSP encerra as conexões VPN e BGP com o gateway RAS. Com essa configuração, o hardware do roteador de terceiros interno deve oferecer suporte à redistribuição de rotas IGP para BGP, bem como redistribuir as rotas BGP para IGP.  
  
![Pontos de encerramento separados para BGP e VPN](../../media/Border-Gateway-Protocol-BGP/bgp_04.jpg)  
  
O roteador interno aprende as rotas corporativas por meio de um dos seguintes mecanismos:  
  
-   BGP  
  
-   Um Interior Gateway Protocol (IGP) como OSPF ou RIP.  
  
-   Configuração de roteamento estático  
  
Quando qualquer IGP é usado no site da empresa, o roteador interno deve redistribuir rotas de IGP no BGP, bem como redistribuir rotas de BGP para rotas de IGP, para manter a conectividade de sub-rede entre redes virtuais CSP e sub-redes locais corporativas.  
  
Com essa implantação, o gateway de RAS corporativo tem uma conexão VPN site a site com o gateway de RAS do CSP, que fornece o gateway de RAS corporativo com as rotas para o gateway CSP. O roteador interno da empresa aprende essa rota para o gateway do CSP usando o iBGP com o gateway de RAS corporativo. Por isso, o roteador interno da empresa é capaz de estabelecer uma sessão de emparelhamento com o roteador BGP do gateway de RAS do CSP.  
  
Deste ponto em diante, o roteador interno da empresa e as informações de roteamento do gateway de RAS do CSP do Exchange. E o roteador de BGP do RAS corporativo aprende as rotas do CSP e as rotas da empresa para rotear fisicamente os pacotes entre as redes.  
  
## <a name="bkmk_features"></a>Recursos do BGP  
A seguir estão os recursos do roteador BGP de gateway de RAS.  
  
**Roteamento BGP como um serviço de função de acesso remoto**. Agora você pode instalar o serviço de função de **Roteamento** da função de servidor de acesso remoto sem instalar o serviço de função **RAS (serviço de acesso remoto)** quando quiser usar o acesso remoto como um roteador LAN BGP.  Isso reduz a superfície de memória do roteador BGP e instala apenas os componentes necessários para o roteamento de BGP dinâmico. O serviço de função de roteamento é útil quando apenas uma VM do roteador BGP é necessária e você não precisa usar o DirectAccess ou a VPN. Além disso, o uso do acesso remoto como um roteador de LAN com BGP fornece as vantagens de roteamento dinâmico do BGP em sua rede interna.  
  
**Estatísticas de BGP (contadores de mensagens, contadores de rota)** . O roteador BGP dá suporte à exibição das mensagens e estatísticas de rota, se necessário, usando o comando **Get-BgpStatistics** do Windows PowerShell.  
  
**Suporte a Equal Cost Multi Path Routing (ECMP)** . O roteador BGP dá suporte à ECMP e pode ter mais de uma rota de custo igual inseridas na tabela e na pilha de roteamento do BGP. A seleção de roteador BGP da rota para a transmissão de pacotes de dados é aleatória com ECMP habilitado.  
  
**Configuração de HoldTime**. O roteador BGP dá suporte à configuração do valor HoldTimer de acordo com seus requisitos de rede. O temporizador pode ser alterado dinamicamente para acomodar a interoperabilidade com dispositivos de terceiros ou para manter um horário máximo específico de tempo limite da sessão de emparelhamento via protocolo BGP.  
  
**Suporte a BGP interno e externo**. O roteador BGP dá suporte ao emparelhamento de iBGP e eBGP. Para configurar qualquer um deles, você deve garantir que o ASNs apropriados são atribuídos aos roteadores BGP local e remoto. Todas as quatro topologias de implantação de BGP empregam o uso do emparelhamento de eBGP, e a quarta topologia usa também o emparelhamento de iBGP.  
  
**Interoperabilidade com soluções de terceiros**. O roteador BGP baseia-se na especificação mais recente do BGP versão 4 e foi testado quanto à interoperabilidade com a maioria dos principais dispositivos de roteamento BGP de terceiros. Para obter mais informações, consulte Request for Comments (RFC) 4271, [Um Border Gateway Protocol 4 (BGP-4)](https://tools.ietf.org/html/rfc4271).  
  
**Suporte a emparelhamento de transporte de IPv4 e IPv6**. O roteador BGP dá suporte ao emparelhamento de IPv4 e IPv6. No entanto, você deve configurar o identificador de BGP como o endereço IPv4 do roteador BGP. Para todas as topologias de implantação do roteador BGP, qualquer um dos dois tipos de emparelhamento (IPV4 / IPv6) pode ser usado.  
  
**Funcionalidade de aprendizagem e anúncio do roteamento unicast de IPv4 e IPv6 (Multiprotocol Network Layer Reachability Information [NLRI])** . Não importa qual transporte você use, o roteador BGP pode trocar rotas IPv4 e IPv6 se a funcionalidade apropriada for anunciado por outros roteadores BGP durante o estabelecimento da sessão. Para configurar o roteamento IPv6, o parâmetro IPv6Routing deve estar habilitado e um endereço IPv6 Global Local deve ser configurado no nível do roteador.  
  
**Emparelhamento de modo misto e de modo passivo**. Você pode configurar sessões de emparelhamento via protocolo BGP no modo misto – onde o roteador BGP atua como o modo de iniciador e Respondente ou passivo, no qual o roteador BGP não inicia o emparelhamento, mas responde a solicitações de entrada. O modo misto é o padrão e é recomendado para emparelhamento via protocolo BGP. Isso se aplica, a menos que você queira usar o modo passivo para fins de depuração e diagnóstico. Para todas as topologias de implantação do roteador BGP, o emparelhamento de modo misto é necessário para permitir reinícios automáticos no caso de eventos de falha.  
  
**Funcionalidade e reconfiguração de atributo da rota**. Você pode adicionar, modificar ou remover os seguintes atributos dos anúncios de rota de entrada e saída do roteador BGP usando as políticas de roteamento BGP Next-Hop, MED, Local-Pref e Community.  
  
**Filtragem de rotas**. O roteador BGP dá suporte à filtragem de anúncios de rota de entrada ou de saída com base em vários atributos de rota como prefixo, intervalo ASN, comunidade e do próximo salto.  
  
**Cliente RR (route-reflector) e RR**. O roteador BGP pode atuar como um refletor de rota e um cliente RR. Isso é útil em topologias complexas nas quais o RR pode simplificar a rede ao formar clusters RR.  
  
**Suporte de atualização de rota**. O roteador BGP dá suporte à atualização de rota e anuncia essa funcionalidade no emparelhamento por padrão. Ele é capaz de enviar um conjunto novo de atualizações de rota quando solicitado por um par por meio de uma mensagem de atualização de rota, bem como enviar uma atualização de rota para atualizar sua tabela de roteamento nos eventos como alterações de política de roteamento para um par. Isso habilita o cenário de alteração ou atualização das políticas de roteamento BGP no Windows Server 2016 sem a necessidade de reiniciar o emparelhamento.  
  
**Suporte à configuração de roteamento estático**. Você pode configurar rotas estáticas ou interfaces no roteador BGP usando o comando do Windows PowerShell **Add-BgpCustomRoute**. As rotas estáticas que você configurar podem ser os prefixos ou o nome das interfaces por meio dos quais as rotas devem ser escolhidas. No entanto, apenas as rotas com saltos seguintes resolvíveis são inseridas nas tabelas de roteamento de BGP e anunciadas para colegas.  
  
**Suporte a roteamento de tráfego**. O roteador BGP dá suporte ao roteamento de trânsito para conexões iBGP a iBGP, conexões iBGP a eBGP, bem como conexões eBGP a eBGP.  
  
**Redução de oscilação de rota**. A redução de oscilação de rota para roteamento BGP no Windows Server 2016 fornece suporte para retardamento de oscilação de rota. Por exemplo, quando uma rota está sendo constantemente anunciada e removida, tornando a tabela de roteamento instável, você pode configurar o roteador BGP para atribuir um peso de retardamento à rota e monitorá-lo quanto a flaps – e, conseqüentemente, suprimir ou cancelar a supressão, conforme necessário. Isso ajuda a manter uma tabela de roteamento estável e menos processamento pelo Roteador BGP.  
  
**Agregação de rota**. A agregação de rota para o roteador BGP fornece a capacidade de configurar rotas de agregação e substituir os anúncios de rota mais granulares por resumo ou rotas de agregação para pares. Isso resulta em um número menor de mensagens de anúncio de rota transmitidas na rede.  
  
> [!NOTE]  
> No System Center, o gateway de RAS é chamado de gateway do Windows Server.  
  

  

