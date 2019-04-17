---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: "Requisitos de resolução de nome de Proxies de servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a94e4de181cd8794d479bbd6695a94658aba0f86
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Requisitos de resolução de nome de Proxies de servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando os computadores cliente na Internet tentarem acessar um aplicativo que é protegido por \(AD FS\) serviços de Federação do Active Directory, o usuário deve primeiro autenticar para o servidor de Federação. Na maioria dos casos, o servidor de Federação geralmente não é acessível diretamente da Internet. Portanto, os computadores cliente Internet devem redirecionados para o proxy do servidor de Federação em vez disso. Você pode fazer o redirecionamento bem-sucedida adicionando os registros de Domain Name System \(DNS\) apropriados para sua zona DNS ou zonas que enfrentam Internet.  
  
O método que você usa para redirecionar os clientes de Internet para o proxy do servidor de Federação depende de como configurar a zona DNS em sua rede de perímetro ou como configurar uma zona DNS que você controla na Internet. Proxies de servidor de Federação são projetados para uso em uma rede do perímetro. Eles redirecionar solicitações de cliente da Internet para servidores de federação com êxito somente quando o DNS foi configurado corretamente em todas as zonas de Internet\ voltados para que você controla. Portanto, a configuração das suas zonas de face Internet\ — se você tiver uma zona DNS serve apenas a rede do perímetro ou uma zona DNS servindo clientes de Internet e a rede do perímetro — é importante.  
  
Este tópico descreve as etapas que você pode tomar para configurar a resolução de nome quando você coloca um proxy de servidor de Federação em sua rede do perímetro. Para determinar quais etapas a seguir, determine qual das seguintes cenários DNS respeite a infraestrutura DNS na rede de perímetro da sua organização. Em seguida, siga as etapas para esse cenário.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zona DNS serve apenas a rede do perímetro  
Nesse cenário, sua organização tem uma ou duas zonas DNS na rede de perímetro, e sua organização não controla as zonas DNS na Internet. Resolução de nomes bem-sucedida para um proxy de servidor de federação na zona DNS que serve apenas o cenário de rede do perímetro depende das seguintes condições:  
  
-   O proxy do servidor de federação deve ter uma configuração no arquivo de hosts para resolver o domínio totalmente qualificado nomear \(FQDN\) da URL de ponto de extremidade do servidor de federação para um endereço IP de um servidor de Federação ou um cluster de servidor de Federação.  
  
-   DNS na rede de perímetro do parceiro de conta deve ser configurado para que o FQDN da URL de ponto de extremidade do servidor de Federação é resolvido para o endereço IP do proxy do servidor de Federação.  
  
A ilustração a seguir e as etapas correspondentes mostram como cada uma dessas condições é obtida para obter um exemplo de determinado. Nesta ilustração, tecnologia de balanceamento de carga de rede Microsoft \(NLB\) fornece uma única, cluster FQDN e uma única, o endereço IP do cluster para um farm de servidores de Federação existente.  
  
![Requisitos de nome](media/adfs2_deploy_single_fs.gif)  
  
Para obter mais informações sobre como configurar um endereço IP do cluster ou um FQDN do cluster usando NLB, consulte [especificando os parâmetros de Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configure o arquivo de hosts do proxy do servidor de Federação  
Como DNS na rede de perímetro está configurado para resolver todas as solicitações para fs.fabrikam.com para o proxy de servidor de federação de conta, o proxy de servidor de federação de parceiro conta tem uma entrada em seu arquivo de hosts local para resolver fs.fabrikam.com para o endereço IP do servidor de Federação conta real \ (ou nome do cluster DNS para a farm\ de servidor de federação) que está conectado à rede corporativa. Isso torna possível para o proxy do servidor de Federação conta resolver o nome do host fs.fabrikam.com para o servidor de federação de conta em vez da mesmo, como ocorreria se ele tentar procurar fs.fabrikam.com usando DNS do perímetro — para que o proxy do servidor de Federação pode se comunicar com o servidor de Federação.  
  
### <a name="2-configure-perimeter-dns"></a>2. Defina o perímetro DNS  
Como há somente um único AD FS nome de host que os computadores cliente são direcionados para — estarem em uma intranet ou na Internet — os computadores cliente na Internet que usam o servidor DNS perímetro devem ser o FQDN para a conta federação servidor \(fs.fabrikam.com\) resolvido para o endereço IP do proxy do servidor de Federação a conta na rede de perímetro. Para que ele pode encaminhar clientes para o proxy do servidor de Federação conta quando eles tentam resolver fs.fabrikam.com, perímetro DNS contém uma zona DNS corp.fabrikam.com limitada com um registro de recurso \(A\) único host para fs \(fs.fabrikam.com\) e o endereço IP do proxy do servidor de Federação a conta na rede de perímetro.  
  
Para obter mais informações sobre como modificar o arquivo de hosts de proxy do servidor de Federação e configurar o DNS na rede de perímetro, consulte [configurar a resolução de nome para um Proxy de servidor de Federação em uma zona DNS que serve apenas a rede do perímetro](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zona DNS servindo clientes de Internet e a rede do perímetro  
Nesse cenário, sua organização controla a zona DNS na rede de perímetro e pelo menos uma zona DNS na Internet. Resolução de nomes bem-sucedida para um proxy de servidor de Federação neste cenário depende das seguintes condições:  
  
-   DNS na zona da Internet do parceiro de conta deve ser configurado para que o FQDN do nome de host do servidor de Federação é resolvido para o endereço IP do proxy do servidor de federação na rede de perímetro.  
  
-   DNS na rede de perímetro do parceiro de conta deve ser configurado para que o FQDN do nome de host do servidor de Federação é resolvido para o endereço IP do servidor de federação na rede corporativa.  
  
A ilustração a seguir e as etapas correspondentes mostram como cada uma dessas condições é obtida para obter um exemplo de determinado.  
  
![Requisitos de nome](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configure o perímetro DNS  
Para esse cenário, pois presume-se que você irá configurar a zona DNS de Internet que você controle a resolver solicita que são feitas para uma URL de ponto de extremidade específico \ (ou seja, fs.fabrikam.com\) para o proxy do servidor de federação na rede de perímetro, você também deve configurar a zona no perímetro DNS para encaminhar essas solicitações para o servidor de federação na rede corporativa.  
  
Para que os clientes possam ser encaminhados para o servidor de federação de conta quando eles tentam resolver fs.fabrikam.com, perímetro DNS é configurado com um registro de recurso \(A\) único host para fs \(fs.fabrikam.com\) e o endereço IP do servidor de Federação a conta na rede corporativa. Isso torna possível para o proxy do servidor de Federação conta resolver o nome do host fs.fabrikam.com para o servidor de federação de conta em vez da mesmo, como ocorreria se ele tentar procurar fs.fabrikam.com usando DNS de Internet — para que o proxy do servidor de Federação pode se comunicar com o servidor de Federação.  
  
### <a name="2-configure-internet-dns"></a>2. Configurar DNS de Internet  
Resolução de nomes para serem bem-sucedidas nesse cenário, todas as solicitações de computadores cliente na Internet para fs.fabrikam.com devem ser resolvidas por zona da Internet DNS que você controla. Consequentemente, você deve configurar sua zona de DNS de Internet para encaminhar as solicitações de cliente para fs.fabrikam.com para o endereço IP do proxy do servidor de Federação a conta na rede de perímetro.  
  
Para obter mais informações sobre como modificar a rede do perímetro e zonas DNS de Internet, veja [configurar a resolução de nomes de um Proxy de servidor de Federação em um DNS zona que serve tanto a perímetro de rede e Internet clientes](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
