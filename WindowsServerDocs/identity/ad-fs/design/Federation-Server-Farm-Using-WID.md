---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: Farm de servidores de federação usando WID
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 41c2179cbd8bf2c6032f233335099b512c02f880
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832497"
---
# <a name="federation-server-farm-using-wid"></a>Farm de servidores de federação usando WID

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

A topologia padrão para os serviços de Federação do Active Directory \(do AD FS\) é um farm de servidores de Federação usando o banco de dados interno do Windows \(WID\). Nessa topologia, o AD FS usa o WID como o repositório para o banco de dados de configuração do AD FS para todos os servidores de federação que ingressaram nesse farm. O farm replica e mantém os dados do Serviço de Federação no banco de dados de configuração em cada servidor no farm. AD FS no Windows Server 2012 R2 permite às organizações com 100 ou menos objetos de confiança para configurar os farms de servidores de Federação usando WID com até 30 servidores.  
  
O ato de criar o primeiro servidor de federação em um farm também cria um novo Serviço de Federação. Quando você usa o WID para o banco de dados de configuração do AD FS, o primeiro servidor de federação que você cria no farm é conhecido como o *servidor de Federação primário*. Isso significa que esse computador é configurado com uma leitura\/gravar cópia de banco de dados de configuração do AD FS.  
  
Todos os outros servidores de federação que você configurar para este farm são denominados *servidores de Federação secundários* porque eles devem replicar todas as alterações são feitas no servidor de Federação primário para a leitura\-apenas cópias do banco de dados de configuração do AD FS que eles armazenam localmente.  
  
> [!IMPORTANT]  
> Recomendamos o uso de pelo menos dois servidores de Federação em uma carga\-configuração equilibrada.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que estão associadas essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   As organizações com 100 ou menos relações de confiança configuradas que precisa fornecer a seus usuários internos \(conectado computadores que estão fisicamente conectados à rede corporativa\) com o logon único\-na \(SSO\) acesso a aplicativos federados ou serviços  
  
-   Organizações que desejam fornecer a seus usuários internos com acesso SSO para o Microsoft Office 365 ou Microsoft Online Services  
  
-   Organizações menores que exigem serviços redundantes e escalonáveis  
  
> [!NOTE]  
> As organizações com bancos de dados maiores devem considerar o uso de [Federation Server Farm usando o SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia de implantação. As organizações com usuários que fazem logon de fora da rede devem considerar usando o [Federation Server Farm usando o WID e Proxies](Federation-Server-Farm-Using-WID-and-Proxies.md) topologia ou o [Federation Server Farm usando o SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Fornece acesso SSO para usuários internos  
  
-   Redundância de dados e o serviço de Federação \(cada servidor de Federação replica alterações em outros servidores de federação no mesmo farm\)  
  
-   WID está incluído no Windows; Portanto, não há necessidade de comprar o SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   Um farm de WID tem um limite de 30 servidores de federação, se você tiver 100 ou menos objetos de confiança.  
  
-   Um farm de WID não oferece suporte a resolução de artefato ou de detecção de reprodução de token \(faz parte do Security Assertion Markup Language \(SAML\) protocolo\).  
  
A tabela a seguir fornece um resumo para o uso de um farm de WID.  Usá-lo a planejar sua implementação.  
  
|| 1 \- 100 relações de confiança RP | Relações de confiança RP mais de 100 |
| --- | --- | --- |
|1 \- 30 AD FS nós|Suportada de WID|Não tem suporte usando o WID - SQL é necessário 
|Mais de 30 AD FS nós|Não tem suporte usando o WID - SQL é necessário|Não tem suporte usando o WID - SQL é necessário  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de posicionamento e a rede de servidor  
Quando você estiver pronto para começar a implantar essa topologia em sua rede, você deve planejar a colocação de todos os servidores de Federação em sua rede corporativa por trás de um balanceamento de carga de rede \(NLB\) host que pode ser configurada para um cluster NLB com cluster dedicado Domain Name System \(DNS\) endereço IP do nome e o cluster.  
  
> [!NOTE]  
> Esse nome DNS de cluster deve corresponder ao nome do serviço de federação, por exemplo, fs.fabrikam.com.  
  
O host NLB pode usar as configurações que são definidas nesse cluster NLB para alocar solicitações de cliente para os servidores de Federação individuais. A ilustração a seguir mostra como a empresa fictícia da Fabrikam, Inc., define a primeira fase da sua implantação usar uma duas\-farm de servidores de Federação do computador \(fs1 e fs2\) com WID e o posicionamento de um servidor DNS e um único host NLB que está conectado à rede corporativa.  
  
![farm de servidores usando o WID](media/FarmWID.gif)  
  
> [!NOTE]  
> Se houver uma falha neste host NLB único, os usuários não poderão acessar a aplicativos federados ou serviços. Adicione mais hosts NLB se seus requisitos comerciais não permitirem a existência de um único ponto de falha.  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de federação, consulte a seção de requisitos de resolução de nome em [requisitos do AD FS](AD-FS-Requirements.md).  
  
## <a name="see-also"></a>Consulte também  
[Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guia de Design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

