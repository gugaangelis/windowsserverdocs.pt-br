---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: Farm de servidores de federação usando SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 66c8bae2fbccca2bf618e46ffd3ccc05cb52f911
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191496"
---
# <a name="federation-server-farm-using-sql-server"></a>Farm de servidores de federação usando SQL Server

Essa topologia para os serviços de Federação do Active Directory \(do AD FS\) difere do farm de servidor de Federação usando o banco de dados interno do Windows \(WID\) topologia de implantação em que ele não replica os dados para cada servidor de federação no farm. Em vez disso, todos os servidores de federação no farm podem ler e gravar dados em um banco de dados comum que é armazenado em um servidor executando o Microsoft SQL Server que está localizado na rede corporativa.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que estão associadas essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   Organizações de grandes porte com mais de 100 relações de confiança que precisa fornecer a seus usuários internos e usuários externos com logon único\-na \(SSO\) acesso a aplicativos federados ou serviços  
  
-   As organizações que já usam o SQL Server e quiser tirar proveito de suas ferramentas existentes e experiência  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Suporte para números maiores de relações de confiança \(mais de 100\)  
  
-   Suporte para a detecção de reprodução de token \(um recurso de segurança\) e a resolução de artefato \(fazem parte do Security Assertion Markup Language \(SAML\) protocolo 2.0\)  
  
-   Ferramentas de suporte para todos os benefícios do SQL Server, como espelhamento de banco de dados, clustering de failover, relatórios e gerenciamento  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   Essa topologia fornece redundância de banco de dados por padrão. Embora um farm de servidores de federação com topologia WID replica automaticamente o banco de dados em cada servidor de federação no farm de WID, o farm de servidores de federação com topologia de SQL Server contém apenas uma cópia do banco de dados  
  
> [!NOTE]  
> SQL Server dá suporte a muitos dados diferentes e opções de redundância de aplicativos, incluindo o clustering de failover, espelhamento de banco de dados e vários tipos diferentes de replicação do SQL Server.  
  
A tecnologia da informação \(IT\) departamento usa espelhamento de banco de dados do SQL Server em alta\-safety \(síncrona\) modo e o clustering de failover para fornecer alta\- suporte de disponibilidade para a instância do SQL Server. Transacional do SQL Server \(par\-à\-par\) e replicação de mesclagem não foi testado pela equipe de produto do AD FS na Microsoft. Para obter mais informações sobre o SQL Server, consulte [visão geral de soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853) ou [selecionando o tipo de replicação apropriada](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versões com suporte do SQL Server  
As seguintes versões do SQL server são compatíveis com o AD FS instalado com o Windows Server 2012:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de posicionamento e a rede de servidor  
Semelhante ao farm de servidores de federação com topologia WID, todos os servidores de federação no farm estão configurados para usar o sistema de nomes de domínio de um cluster \(DNS\) nome \(que representa o nome do serviço de Federação\)e o endereço IP de um cluster como parte do balanceamento de carga de rede \(NLB\) configuração de cluster. Isso ajuda a host NLB alocar solicitações de cliente para os servidores de Federação individuais. Proxies de servidor de federação podem ser usado para solicitações de cliente de proxy para o farm de servidores de Federação.  
  
A ilustração a seguir mostra como a empresa fictícia Contoso Pharmaceuticals implantados seu farm de servidores de federação com topologia de SQL Server na rede corporativa. Ele também mostra como essa empresa configurou a rede de perímetro com acesso a um servidor DNS, um host NLB adicional que usa o mesmo nome DNS de cluster \(fs.contoso.com\) que é usado no cluster NLB de rede corporativa e com dois proxies de servidor de Federação \(fsp1 e fsp2\).  
  
![farm de servidores usando o SQL](media/FarmSQLProxies.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies do servidor de federação, consulte [requisitos de resolução de nome para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) ou [nome Requisitos de resolução para Proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
