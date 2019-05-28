---
ms.assetid: 663a2482-33d1-4c19-8607-2e24eef89fcb
title: Farm de servidores de federação usando WID
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f0f3d8c70a41d512e7cd33282524bc401ce84600
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191433"
---
# <a name="federation-server-farm-using-wid"></a>Farm de servidores de federação usando WID

A topologia padrão para os serviços de Federação do Active Directory \(do AD FS\) é um farm de servidores de Federação usando o banco de dados interno do Windows \(WID\), que consiste em até cinco servidores de federação que hospedam seu Serviço de Federação da organização. Nessa topologia, o AD FS usa o WID como o repositório para o banco de dados de configuração do AD FS para todos os servidores de federação que ingressaram nesse farm. O farm replica e mantém os dados do Serviço de Federação no banco de dados de configuração em cada servidor no farm.  
  
O ato de criar o primeiro servidor de federação em um farm também cria um novo Serviço de Federação. Quando você usa o WID para o banco de dados de configuração do AD FS, o primeiro servidor de federação que você cria no farm é conhecido como o *servidor de Federação primário*. Isso significa que esse computador é configurado com uma leitura\/gravar cópia de banco de dados de configuração do AD FS.  
  
Todos os outros servidores de federação que você configurar para este farm são denominados *servidores de Federação secundários* porque eles devem replicar todas as alterações são feitas no servidor de Federação primário para a leitura\-apenas cópias do banco de dados de configuração do AD FS que eles armazenam localmente.  
  
> [!NOTE]  
> Recomendamos o uso de pelo menos dois servidores de Federação em uma carga\-configuração equilibrada.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que estão associadas essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   As organizações com 100 ou menos relações de confiança configuradas que precisa fornecer a seus usuários internos \(conectado computadores que estão fisicamente conectados à rede corporativa\) com o logon único\-na \(SSO\) acesso a aplicativos federados ou serviços  
  
-   Organizações que desejam fornecer a seus usuários internos com acesso SSO para o Microsoft Office 365 ou Microsoft Online Services  
  
-   Organizações menores que exigem serviços redundantes e escalonáveis  
  
> [!NOTE]  
> As organizações com bancos de dados maiores devem considerar o uso de [Federation Server Farm usando o SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia de implantação, que é descrita posteriormente nesta seção. As organizações com usuários que fazem logon de fora da rede devem considerar usando o [Federation Server Farm usando o WID e Proxies](Federation-Server-Farm-Using-WID-and-Proxies.md) topologia ou o [Federation Server Farm usando o SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Fornece acesso SSO para usuários internos  
  
-   Redundância de dados e o serviço de Federação \(cada servidor de Federação replica alterações em outros servidores de federação no mesmo farm\)  
  
-   O farm pode ser escalado horizontalmente pela adição de até cinco servidores de Federação  
  
-   WID está incluído no Windows; Portanto, não há necessidade de comprar o SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   Um farm de WID tem um limite de cinco servidores de Federação. Para mais informações, consulte [Considerações de topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
-   Um farm de WID não oferece suporte a resolução de artefato ou de detecção de reprodução de token \(faz parte do Security Assertion Markup Language \(SAML\) protocolo\).  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de posicionamento e a rede de servidor  
Quando você estiver pronto para começar a implantar essa topologia em sua rede, você deve planejar a colocação de todos os servidores de Federação em sua rede corporativa por trás de um balanceamento de carga de rede \(NLB\) host que pode ser configurada para um cluster NLB com cluster dedicado Domain Name System \(DNS\) endereço IP do nome e o cluster.  
  
> [!NOTE]  
> Esse nome DNS de cluster deve corresponder ao nome do serviço de federação, por exemplo, fs.fabrikam.com.  
  
O host NLB pode usar as configurações que são definidas nesse cluster NLB para alocar solicitações de cliente para os servidores de Federação individuais. A ilustração a seguir mostra como a empresa fictícia da Fabrikam, Inc., define a primeira fase da sua implantação usar uma duas\-farm de servidores de Federação do computador \(fs1 e fs2\) com WID e o posicionamento de um servidor DNS e um único host NLB que está conectado à rede corporativa.  
  
![farm de servidores usando o WID](media/FarmWID.gif)  
  
> [!NOTE]  
> Se houver uma falha neste host NLB único, os usuários não poderão acessar a aplicativos federados ou serviços. Adicione mais hosts NLB se seus requisitos comerciais não permitirem a existência de um único ponto de falha.  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de federação, consulte [requisitos de resolução de nome para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) no guia de Design do AD FS.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
