---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: Requisitos de resolução de nome para proxies de servidor de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 51176101b471ec940e2b43a95e1a1a8d37b394f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408063"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Requisitos de resolução de nome para proxies de servidor de federação

Quando os computadores cliente na Internet tentam acessar um aplicativo protegido por Serviços de Federação do Active Directory (AD FS) \(AD FS\), eles devem primeiro se autenticar no servidor de Federação. Na maioria dos casos, o servidor de Federação geralmente não pode ser acessado diretamente da Internet. Portanto, os computadores cliente da Internet devem ser redirecionados para o proxy do servidor de Federação. Você pode realizar o redirecionamento bem-sucedido adicionando o sistema de nomes de domínio apropriado \(registros de\) DNS em sua zona DNS ou zonas que enfrentam a Internet.  
  
O método usado para redirecionar clientes da Internet para o proxy do servidor de Federação depende de como você configura a zona DNS em sua rede de perímetro ou como configura uma zona DNS que você controla na Internet. Proxies do servidor de Federação são destinados a uso em uma rede de perímetro. Eles redirecionam as solicitações de cliente da Internet para servidores de Federação somente com êxito quando o DNS tiver sido configurado corretamente em todas as zonas de Internet\-voltadas para o controle. Portanto, a configuração de sua Internet\-zonas opostas – se você tem uma zona DNS servindo apenas a rede de perímetro ou uma zona DNS que atende tanto à rede de perímetro quanto aos clientes da Internet — é importante.  
  
Este tópico descreve as etapas que você pode executar para configurar a resolução de nomes ao colocar um proxy de servidor de Federação em sua rede de perímetro. Para determinar quais etapas seguir, determine qual dos seguintes cenários DNS mais corresponde com a infraestrutura do DNS na rede de perímetro da sua organização. Então siga as etapas para esse cenário.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zona DNS atendendo apenas a rede de perímetro  
Nesse cenário, sua organização tem uma ou duas zonas DNS na rede de perímetro, e sua organização não controla as zonas DNS na Internet. A resolução de nomes bem-sucedida para um proxy de servidor de Federação na zona DNS que atende apenas ao cenário de rede de perímetro depende das seguintes condições:  
  
-   O proxy do servidor de Federação deve ter uma configuração no arquivo de hosts para resolver o nome de domínio totalmente qualificado \(FQDN\) da URL do ponto de extremidade do servidor de Federação para um endereço IP de um servidor de Federação ou um cluster de servidor de Federação.  
  
-   O DNS na rede de perímetro do parceiro de conta deve ser configurado para que o FQDN da URL do ponto de extremidade do servidor de Federação seja resolvido para o endereço IP do proxy do servidor de Federação.  
  
A ilustração a seguir e as etapas correspondentes mostram como cada uma dessas condições é obtida para um determinado exemplo. Nesta ilustração, o balanceamento de carga de rede da Microsoft \(tecnologia de\) NLB fornece um único FQDN de cluster e um único endereço IP de cluster para um farm de servidores de Federação existente.  
  
![requisitos de nome](media/adfs2_deploy_single_fs.gif)  
  
Para obter mais informações sobre como configurar um endereço IP de cluster ou um FQDN de cluster usando NLB, consulte [especificando os parâmetros de cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurar o arquivo de hosts no proxy do servidor de federação  
Como o DNS na rede de perímetro está configurado para resolver todas as solicitações de fs.fabrikam.com para o proxy do servidor de Federação da conta, o proxy do servidor de Federação do parceiro de conta tem uma entrada em seu arquivo hosts local para resolver fs.fabrikam.com para o endereço IP do servidor de Federação da conta real \(ou nome DNS do cluster para o\) do farm de servidores de Federação que está conectado à rede corporativa Isso possibilita que o proxy de servidor de Federação de conta resolva o nome de host fs.fabrikam.com para o servidor de Federação de conta em vez de ser o próprio — como ocorreria se tentasse Pesquisar fs.fabrikam.com usando o DNS de perímetro — para que a Federação o proxy do servidor pode se comunicar com o servidor de Federação.  
  
### <a name="2-configure-perimeter-dns"></a>2. Configurar DNS de perímetro  
Como há apenas um único nome de host AD FS para o qual os computadores cliente são direcionados — estejam eles em uma intranet ou na Internet — os computadores cliente na Internet que usam o servidor DNS de perímetro devem resolver o FQDN do servidor de Federação de conta \(fs.fabrikam.com\) para o endereço IP do proxy de servidor de Federação de conta na rede de perímetro. Para que ele possa encaminhar clientes para o proxy de servidor de Federação de conta ao tentar resolver fs.fabrikam.com, o DNS de perímetro contém uma zona DNS corp.fabrikam.com limitada com um único host \(um registro de recurso\) para FS \(fs.fabrikam.com\) e o endereço IP do proxy de servidor de Federação de conta na rede de perímetro.  
  
Para obter mais informações sobre como modificar o arquivo de hosts do proxy do servidor de Federação e configurar o DNS na rede de perímetro, consulte [Configurar a resolução de nomes para um proxy de servidor de Federação em uma zona DNS que serve apenas para a rede de perímetro](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zona DNS atendendo a rede de perímetro e os clientes da Internet  
Nesse cenário, a sua organização controla a zona DNS na rede de perímetro e pelo menos uma zona DNS na Internet. A resolução de nomes bem-sucedida para um proxy de servidor de Federação neste cenário depende das seguintes condições:  
  
-   O DNS na zona da Internet do parceiro de conta deve ser configurado para que o FQDN do nome de host do servidor de Federação seja resolvido para o endereço IP do proxy do servidor de Federação na rede de perímetro.  
  
-   O DNS na rede de perímetro do parceiro de conta deve ser configurado para que o FQDN do nome de host do servidor de Federação seja resolvido para o endereço IP do servidor de Federação na rede corporativa.  
  
A ilustração a seguir e as etapas correspondentes mostram como cada uma dessas condições é obtida para um determinado exemplo.  
  
![requisitos de nome](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configurar DNS de perímetro  
Para esse cenário, como supõe-se que você configurará a zona DNS da Internet que você controla para resolver solicitações feitas para uma URL de ponto de extremidade específica \(ou seja, fs.fabrikam.com\) ao proxy do servidor de Federação na rede de perímetro, você também deve configurar a zona no DNS de perímetro para encaminhar essas solicitações para o servidor de Federação na rede corporativa.  
  
Para que os clientes possam ser encaminhados para o servidor de Federação de conta quando tentarem resolver fs.fabrikam.com, o DNS de perímetro é configurado com um único host \(um registro de recurso\) para FS \(fs.fabrikam.com\) e o endereço IP do servidor de Federação da conta na rede corporativa. Isso possibilita que o proxy de servidor de Federação de conta resolva o nome de host fs.fabrikam.com para o servidor de Federação de conta em vez de ser o próprio — como ocorreria se tentasse procurar fs.fabrikam.com usando o DNS da Internet — para que o servidor de Federação o proxy pode se comunicar com o servidor de Federação.  
  
### <a name="2-configure-internet-dns"></a>2. Configurar DNS da Internet  
Para a resolução de nome ser bem-sucedida neste cenário, todas as solicitações de computadores cliente na Internet para fs.fabrikam.com devem ser resolvidas pela zona DNS da Internet que você controla. Consequentemente, você deve configurar sua zona DNS de Internet para encaminhar solicitações de cliente para fs.fabrikam.com para o endereço IP do proxy de servidor de Federação de conta na rede de perímetro.  
  
Para obter mais informações sobre como modificar a rede de perímetro e as zonas DNS da Internet, consulte [Configurar a resolução de nomes para um proxy de servidor de Federação em uma zona DNS que serve tanto a rede de perímetro quanto os clientes da Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
