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
ms.openlocfilehash: 2298ba360bb59c950a78514d137c0b4d5fbdd1af
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867818"
---
# <a name="federation-server-farm-using-wid"></a>Farm de servidores de federação usando WID

A topologia padrão para serviços de Federação do Active Directory (AD FS) \(AD FS\) é um farm de servidores de Federação, usando o wid \(\)do banco de dados interno do Windows, que consiste em até cinco servidores de Federação que hospedam seu Serviço de Federação da organização. Nessa topologia, AD FS usa WID como o repositório para o banco de dados de configuração de AD FS para todos os servidores de Federação que ingressaram nesse farm. O farm replica e mantém os dados do Serviço de Federação no banco de dados de configuração em cada servidor no farm.  
  
O ato de criar o primeiro servidor de federação em um farm também cria um novo Serviço de Federação. Quando você usa o WID para o banco de dados de configuração do AD FS, o primeiro servidor de Federação que você cria no farm é chamado de *servidor de Federação primário*. Isso significa que esse computador está configurado com uma cópia\/de leitura/gravação do banco de dados de configuração do AD FS.  
  
Todos os outros servidores de Federação configurados para esse farm são chamados de *servidores de Federação secundários* , pois eles devem replicar quaisquer alterações feitas no servidor de Federação primário para\-as cópias somente leitura do AD FS banco de dados de configuração que eles armazenam localmente.  
  
> [!NOTE]  
> Recomendamos o uso de pelo menos dois servidores de Federação em uma\-configuração de balanceamento de carga.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve as várias considerações sobre o público-alvo, os benefícios e as limitações associados a essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   Organizações com 100 ou menos relações de confiança configuradas que precisam fornecer seus \(usuários internos conectados a computadores que estão conectados fisicamente à rede\) corporativa com logon\-único \(Acesso\) SSO a aplicativos ou serviços federados  
  
-   Organizações que desejam fornecer aos usuários internos acesso de SSO ao Microsoft Online Services ou Microsoft Office 365  
  
-   Organizações menores que exigem serviços redundantes e escalonáveis  
  
> [!NOTE]  
> As organizações com bancos de dados maiores devem considerar o uso do [farm de servidores de Federação usando SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia de implantação, que é descrita mais adiante nesta seção. As organizações com usuários que fazem logon de fora da rede devem considerar o uso do [farm de servidores de Federação usando a topologia wid e proxies](Federation-Server-Farm-Using-WID-and-Proxies.md) ou o farm de servidores de [Federação usando](Federation-Server-Farm-Using-SQL-Server.md) a topologia SQL Server.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Fornece acesso de SSO a usuários internos  
  
-   Redundância de dados e serviço de Federação \(cada servidor de Federação Replica as alterações para outros servidores de Federação no mesmo farm\)  
  
-   O farm pode ser escalado horizontalmente adicionando até cinco servidores de Federação  
  
-   O WID está incluído no Windows; Portanto, não é necessário comprar SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   Um farm WID tem um limite de cinco servidores de Federação. Para mais informações, consulte [Considerações de topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
-   Um farm wid não dá suporte à detecção de reprodução de token ou \(parte da resolução de \(artefatos\)do protocolo SAML\) Security Assertion Markup Language.  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento do servidor  
Quando estiver pronto para começar a implantar essa topologia em sua rede, você deve planejar colocar todos os servidores de Federação em sua rede corporativa atrás de um host NLB \(\) de balanceamento de carga de rede que possa ser configurado para um cluster NLB com um nome DNS \(\) do sistema de nome de domínio do cluster dedicado e um endereço IP do cluster.  
  
> [!NOTE]  
> Esse nome DNS do cluster deve corresponder ao nome do Serviço de Federação, por exemplo, fs.fabrikam.com.  
  
O host NLB pode usar as configurações definidas nesse cluster NLB para alocar solicitações de cliente para os servidores de Federação individuais. A ilustração a seguir mostra como a empresa fictícia Fabrikam, Inc., configura a primeira fase de sua implantação usando um farm\- \(de servidores de Federação de dois computadores\) FS1 e FS2 com wid e o posicionamento de um servidor DNS e um único host NLB com conexão com a rede corporativa.  
  
![farm de servidores usando WID](media/FarmWID.gif)  
  
> [!NOTE]  
> Se houver uma falha neste host NLB único, os usuários não poderão acessar aplicativos ou serviços federados. Adicione mais hosts NLB se seus requisitos comerciais não permitirem a existência de um único ponto de falha.  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação, consulte [requisitos de resolução de nomes para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) no guia de Design de AD FS.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
