---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: "Farm de servidores de Federação usando o SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a0fff975b9cb278e59686323d2bd72e641597573
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>Farm de servidores de Federação usando o SQL Server

>Aplica-se a: Windows Server 2012

Essa topologia para serviços de Federação do Active Directory \(AD FS\) difere farm de servidor de Federação usando topologia de implantação do banco de dados interno do Windows \(WID\) que ele não replica os dados para cada servidor de federação no farm. Em vez disso, todos os servidores de federação no farm podem ler e gravar dados em um banco de dados comum que é armazenado em um servidor que executa o Microsoft SQL Server que está localizado na rede corporativa.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que são associadas essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   Grandes organizações com mais de 100 relações de confiança que precisam fornecer seus usuários internos e usuários externos único sign\ em \(SSO\) acesso a serviços ou aplicativo federado  
  
-   As organizações que já usam o SQL Server e querem aproveitar suas ferramentas existentes e experiência  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são as vantagens de usar essa topologia?  
  
-   Suporte para um número maior de relações de confiança \(more than 100\)  
  
-   Suporte para detecção de repetição token \(a security feature\) e resolução de artefato \ (parte do \(SAML\) a linguagem de marcação de asserção segurança 2.0 protocol\)  
  
-   Ferramentas de suporte para todos os benefícios do SQL Server, como o espelhamento de banco de dados, cluster de failover, relatórios e gerenciamento  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações de usar essa topologia?  
  
-   Essa topologia não fornece redundância do banco de dados por padrão. Embora um farm de servidores de federação com topologia de trabalho replica automaticamente o banco de dados de trabalho em cada servidor de federação no farm, o farm de servidores de federação com topologia do SQL Server contém apenas uma cópia do banco de dados  
  
> [!NOTE]  
> SQL Server dá suporte a muitos dados diferentes e opções de redundância de aplicativos, incluindo cluster de failover, espelhamento de banco de dados e vários tipos diferentes de replicação do SQL Server.  
  
O departamento de tecnologia da informação Microsoft \(IT\) usa o espelhamento de banco de dados do SQL Server no modo de \(synchronous\) de proteção para a high\ e failover clustering para oferecer suporte a disponibilidade de high\ para a instância do SQL Server. SQL Server transacionais \(peer\-to\-peer\) e mesclar a replicação não foram testados pela equipe de produto do AD FS na Microsoft. Para obter mais informações sobre o SQL Server, consulte [visão geral de soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853) ou [selecionando o tipo apropriado de replicação](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versões compatíveis do SQL Server  
As seguintes versões do servidor SQL são compatíveis com o AD FS instalada com o Windows Server 2012:  
  
-   SQL Server 2008 \ / R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento de servidor  
Assim como o farm de servidores de federação com topologia de trabalho, todos os servidores de federação no farm estão configurados para usar um nome do cluster \(DNS\) Domain Name System \ (que representa o serviço de Federação name\) e o endereço IP de um cluster como parte da configuração do cluster \(NLB\) balanceamento de carga de rede. Isso ajuda o host NLB alocar solicitações de cliente para os servidores de Federação individuais. Proxies de servidor de federação podem ser usado para solicitações de cliente de proxy para o farm de servidores de Federação.  
  
A ilustração a seguir mostra como a empresa fictícia Contoso produtos farmacêuticos implantado seu farm de servidores de federação com topologia do SQL Server na rede corporativa. Ele também mostra como essa empresa configurado a rede do perímetro com acesso a um servidor DNS, um host NLB adicional que usa o mesmo cluster DNS nome \(fs.contoso.com\) que é usado no cluster NLB rede corporativa, e com dois proxies de servidor de Federação \(fsp1 and fsp2\).  
  
![Farm de servidores usando o SQL](media/FarmSQLProxies.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies de servidor de federação, consulte [requisitos de resolução de nome para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) ou [requisitos de resolução de nome de Proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
