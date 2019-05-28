---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: Requisitos de resolução de nome para proxies de servidor de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8aef8b3d8f1e6dde4f960a3bee5a93964d07c72b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191279"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Requisitos de resolução de nome para proxies de servidor de federação

Quando os computadores cliente na Internet tentam acessar um aplicativo que é protegido pelos serviços de Federação do Active Directory \(do AD FS\), eles devem primeiro autenticar para o servidor de Federação. Na maioria dos casos, o servidor de Federação geralmente não é diretamente acessível pela Internet. Portanto, computadores cliente da Internet deverá ser redirecionados em vez disso, para o proxy do servidor de Federação. Você pode realizar o redirecionamento bem-sucedido, adicionando o sistema de nome de domínio apropriado \(DNS\) registros para a zona DNS ou zonas voltadas para a Internet.  
  
O método que você usa para redirecionar os clientes da Internet para o proxy do servidor de Federação depende de como configurar a zona DNS na rede de perímetro ou como configurar uma zona DNS que você controle na Internet. Proxies do servidor de Federação são destinados a uso em uma rede de perímetro. Eles redirecionar as solicitações de cliente da Internet para servidores de federação com êxito somente quando o DNS foi configurada corretamente em todos os Internet\-voltada para as zonas que você controla. Portanto, a configuração de sua Internet\-voltada para as zonas — se você tem uma zona DNS atendendo apenas a rede de perímetro ou em uma zona DNS atendendo a rede de perímetro e os clientes da Internet — é importante.  
  
Este tópico descreve as etapas que você pode executar para configurar a resolução de nome quando você coloca um proxy do servidor de federação na rede de perímetro. Para determinar quais etapas seguir, determine qual dos seguintes cenários DNS mais corresponde com a infraestrutura do DNS na rede de perímetro da sua organização. Então siga as etapas para esse cenário.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zona DNS atendendo apenas a rede de perímetro  
Nesse cenário, sua organização tem uma ou duas zonas DNS na rede de perímetro, e sua organização não controla as zonas DNS na Internet. Resolução de nomes bem-sucedida para um proxy do servidor de federação na zona DNS que serve apenas o cenário de rede de perímetro depende das seguintes condições:  
  
-   O proxy do servidor de federação deve ter uma configuração no arquivo de hosts para resolver o nome de domínio totalmente qualificado \(FQDN\) da URL do ponto de extremidade do servidor de federação para um endereço IP de um servidor de Federação ou um cluster de servidor de Federação.  
  
-   DNS na rede de perímetro do parceiro de conta deve ser configurado para que o FQDN da URL de ponto de extremidade do servidor de Federação resolve o endereço IP do proxy de servidor de Federação.  
  
A ilustração a seguir e as etapas correspondentes mostram como cada uma dessas condições é obtida para um determinado exemplo. Nesta ilustração, balanceamento de carga de rede da Microsoft \(NLB\) tecnologia fornece um único FQDN de cluster e um único endereço IP do cluster para um farm de servidor de Federação existente.  
  
![requisitos de nome](media/adfs2_deploy_single_fs.gif)  
  
Para obter mais informações sobre como configurar um endereço IP ou FQDN de cluster usando NLB, consulte [especificando os parâmetros de Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurar o arquivo de hosts no proxy do servidor de federação  
Como o DNS na rede de perímetro é configurado para resolver todas as solicitações de fs.fabrikam.com para o proxy de servidor de federação de conta, o proxy de servidor de federação de parceiro de conta tem uma entrada em seu arquivo de hosts local para resolver fs.fabrikam.com para o endereço IP das servidor de federação de conta reais \(ou o nome DNS de cluster para o farm de servidores de Federação\) que está conectado à rede corporativa. Isso torna possível para o proxy de servidor de federação de conta resolver o nome de host fs.fabrikam.com para o servidor de federação de conta em vez de em si – como ocorreria se tentasse consultar FS.Fabrikam.com usando o DNS de perímetro — para que a federação proxy do servidor pode se comunicar com o servidor de Federação.  
  
### <a name="2-configure-perimeter-dns"></a>2. Configurar DNS de perímetro  
Porque há apenas um único AD FS nome de host que computadores cliente são direcionados a — se eles estão em uma intranet ou na Internet — computadores cliente na Internet que usam o servidor DNS de perímetro devem resolver o FQDN para o servidor de federação de conta \(fs.fabrikam.com\) para o endereço IP do proxy do servidor de federação de conta na rede de perímetro. Para que ele possa encaminhar os clientes para o proxy de servidor de federação de conta quando eles tentam resolver fs.fabrikam.com, perímetro DNS contém uma zona DNS corp.fabrikam.com limitada com um único host \(um\) registro de recurso para fs \(fs.fabrikam.com\) e o endereço IP do proxy do servidor de federação de conta na rede de perímetro.  
  
Para obter mais informações sobre como modificar o arquivo de hosts de proxy do servidor de Federação e configurar o DNS na rede de perímetro, consulte [configurar a resolução de nomes para um Proxy do servidor de Federação em uma zona DNS que atende apenas a rede de perímetro](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zona DNS atendendo a rede de perímetro e os clientes da Internet  
Nesse cenário, a sua organização controla a zona DNS na rede de perímetro e pelo menos uma zona DNS na Internet. Resolução de nomes bem-sucedida para um proxy do servidor de Federação neste cenário depende das seguintes condições:  
  
-   DNS na zona da Internet do parceiro de conta deve ser configurado para que o FQDN do nome de host do servidor de Federação resolve o endereço IP de proxy do servidor de federação na rede de perímetro.  
  
-   DNS na rede de perímetro do parceiro de conta deve ser configurado para que o FQDN do nome de host do servidor de Federação resolve o endereço IP do servidor de federação na rede corporativa.  
  
A ilustração a seguir e as etapas correspondentes mostram como cada uma dessas condições é obtida para um determinado exemplo.  
  
![requisitos de nome](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configurar DNS de perímetro  
Para este cenário, porque se presume que você irá configurar a zona DNS de Internet que você controla para resolver as solicitações que são feitas para uma URL de ponto de extremidade específico \(ou seja, fs.fabrikam.com\) para o proxy de servidor de federação no rede de perímetro, você também deve configurar a zona no perímetro DNS para encaminhar essas solicitações para o servidor de federação na rede corporativa.  
  
Para que os clientes possam ser encaminhados para o servidor de federação de conta quando eles tentam resolver fs.fabrikam.com, perímetro DNS está configurado com um único host \(um\) registro de recurso para fs \(fs.fabrikam.com\) e o endereço IP do servidor de federação de conta na rede corporativa. Isso torna possível para o proxy de servidor de federação de conta resolver o nome de host fs.fabrikam.com para o servidor de federação de conta em vez de em si – como ocorreria se tentasse consultar FS.Fabrikam.com usando o DNS da Internet — para que o servidor de Federação proxy pode se comunicar com o servidor de Federação.  
  
### <a name="2-configure-internet-dns"></a>2. Configurar DNS da Internet  
Para a resolução de nome ser bem-sucedida neste cenário, todas as solicitações de computadores cliente na Internet para fs.fabrikam.com devem ser resolvidas pela zona DNS da Internet que você controla. Consequentemente, você deve configurar a zona DNS da Internet para encaminhar solicitações de cliente para fs.fabrikam.com para o endereço IP do proxy do servidor de federação de conta na rede de perímetro.  
  
Para obter mais informações sobre como modificar a rede de perímetro e zonas de DNS da Internet, consulte [configurar a resolução de nomes para um Proxy do servidor de Federação em um DNS zona que serve tanto a rede de perímetro e os clientes da Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
