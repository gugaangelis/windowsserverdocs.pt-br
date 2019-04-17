---
ms.assetid: 663a2482-33d1-4c19-8607-2e24eef89fcb
title: "Farm de servidores de Federação usando o trabalho"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bb4e5f88f3d62511b185a2b4317416169717c860
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid"></a>Farm de servidores de Federação usando o trabalho

>Aplica-se a: Windows Server 2012

A topologia padrão para serviços de Federação do Active Directory \(AD FS\) é um farm de servidores de federação, usando o \(WID\) banco de dados interno do Windows, que consiste em até cinco servidores de federação que hospedam o serviço de Federação da sua organização. Nessa topologia, o AD FS usa trabalho como a loja para o banco de dados de configuração do AD FS para todos os servidores de federação que fazem parte de farm. O farm replica e mantém os dados do serviço de federação no banco de dados de configuração em cada servidor no farm.  
  
O ato de criar o primeiro servidor de Federação em um farm também cria um novo serviço de Federação. Quando você usa o trabalho para o banco de dados de configuração do AD FS, o primeiro servidor de federação que você criar no farm é conhecido como o *servidor de Federação principal*. Isso significa que este computador está configurado com uma cópia read\/gravação do banco de dados de configuração do AD FS.  
  
Todos os outros servidores de federação que você definir para essa farm são chamados de *servidores de Federação secundário* porque eles devem replicar as alterações feitas no servidor de Federação principal para as cópias somente read\ do AD FS configuração banco de dados armazenados localmente por eles.  
  
> [!NOTE]  
> Recomendamos o uso de pelo menos dois servidores de Federação em uma configuração equilibrado load\.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que são associadas essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   As organizações com 100 ou menos relações de confiança configurado que precisam fornecer a seus usuários internos \ (fazer logon em computadores que estejam conectados fisicamente para a rede \ corporativa) único sign\ em \(SSO\) acesso a serviços ou aplicativos federados  
  
-   Organizações que desejam fornecer a seus usuários internos com acesso SSO para Serviços Online da Microsoft ou do Microsoft Office 365  
  
-   Organizações menores que exigem serviços redundantes, escalonáveis  
  
> [!NOTE]  
> As organizações com bancos de dados maiores devem considerar o uso do [federação Server Farm usando o SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia de implantação, que é descrita mais adiante nesta seção. As organizações com usuários que fazem logon de fora da rede devem considerar usando o [federação Server Farm usando o trabalho e Proxies](Federation-Server-Farm-Using-WID-and-Proxies.md) topologia ou o [federação Server Farm usando o SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são as vantagens de usar essa topologia?  
  
-   Fornece acesso de logon único para usuários internos  
  
-   Dados e o serviço de Federação redundância \ (cada servidor de Federação replica alterações para outros servidores de federação na mesma farm\)  
  
-   O farm pode ser dimensionado com a adição de até cinco servidores de Federação  
  
-   Trabalho está incluído no Windows; Portanto, não há necessidade de adquirir SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações de usar essa topologia?  
  
-   Um farm de trabalho tem um limite de cinco servidores de Federação. Para obter mais informações, consulte [considerações de topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
-   Um farm de trabalho não oferece suporte a resolução de detecção ou artefato de repetição token \ (parte do \(SAML\) protocol\ a segurança asserção Markup Language).  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento de servidor  
Quando você estiver pronto para iniciar a implantação essa topologia em sua rede, você deve planejar colocar todos os servidores de Federação em sua rede corporativa por trás de um host de balanceamento de carga de rede \(NLB\) que pode ser configurado para um cluster NLB com um nome de Domain Name System \(DNS\) de cluster dedicado e o endereço IP do cluster.  
  
> [!NOTE]  
> Esse nome DNS de cluster deve corresponder ao nome do serviço de federação, por exemplo, fs.fabrikam.com.  
  
O host NLB pode usar as configurações que são definidas no cluster NLB para alocar solicitações de cliente para os servidores de Federação individuais. A ilustração a seguir mostra como empresa fictícia Fabrikam, Inc., configura a primeira fase da sua implantação usando um farm de servidores de Federação do computador two\ \(fs1 and fs2\) com o trabalho e o posicionamento de um servidor DNS e um único host NLB está conectado à rede corporativa.  
  
![Farm de servidores usando o trabalho](media/FarmWID.gif)  
  
> [!NOTE]  
> Se houver uma falha nesse host NLB único, os usuários não serão capazes de acessar aplicativos federados ou serviços. Adicione outros hosts NLB se seus requisitos de negócios não permitir que ter um único ponto de falha.  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de federação, consulte [requisitos de resolução de nome para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) no guia de Design do AD FS.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
