---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: AD FS farm de servidores de Federação usando SQL Server
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3c796dcdeaf5fa3dfcd85e7f7db850e5ca85ef4f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942750"
---
# <a name="federation-server-farm-using-sql-server"></a>Farm de servidores de federação usando SQL Server

Essa topologia para Serviços de Federação do Active Directory (AD FS) \( AD FS \) difere do farm de servidores de Federação usando a topologia de implantação de wid do banco de \( \) dados interno do Windows, pois ela não replica o dado para cada servidor de Federação no farm. Em vez disso, todos os servidores de Federação no farm podem ler e gravar dados em um banco de dado comum que é armazenado em um servidor que executa Microsoft SQL Server que está localizado na rede corporativa.

## <a name="deployment-considerations"></a>Considerações de implantação
Esta seção descreve as várias considerações sobre o público-alvo, os benefícios e as limitações associados a essa topologia de implantação.

### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?

-   Grandes organizações com mais de 100 relações de confiança que precisam fornecer os usuários internos e usuários externos com \- acesso SSO de logon \( único \) a aplicativos ou serviços federados

-   Organizações que já usam SQL Server e desejam aproveitar suas ferramentas e especialistas existentes

### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?

-   Suporte para números maiores de relações de confiança \( mais de 100\)

-   Suporte para detecção de token de reprodução \( um recurso \) de segurança e uma \( parte de resolução do artefato do Security Assertion Markup Language \( \) protocolo SAML 2,0\)

-   Suporte para todos os benefícios do SQL Server, como espelhamento de banco de dados, clustering de failover, relatórios e ferramentas de gerenciamento

### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?

-   Essa topologia não fornece redundância de banco de dados por padrão. Embora um farm de servidores de Federação com topologia de WID replique automaticamente o banco de dados WID em cada servidor de Federação no farm, o farm de servidores de Federação com SQL Server topologia contém apenas uma cópia do banco de dados

> [!NOTE]
> O SQL Server dá suporte a muitos dados e opções de redundância de aplicativo diferentes, incluindo clustering de failover, espelhamento de banco de dado e vários tipos diferentes de replicação de SQL Server.

O departamento de ti da tecnologia de informações da Microsoft \( \) usa SQL Server espelhamento de banco de dados no \- modo síncrono de alta segurança \( \) e clustering de failover para fornecer suporte de alta \- disponibilidade para a instância de SQL Server. SQL Server ponto transacional \( \- para \- ponto \) e replicação de mesclagem não foram testados pela equipe de produto do AD FS na Microsoft. Para obter mais informações sobre SQL Server, consulte [visão geral das soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853) ou [selecionando o tipo apropriado de replicação](https://go.microsoft.com/fwlink/?LinkId=214648).

### <a name="supported-sql-server-versions"></a>Versões SQL Server com suporte
As seguintes versões do SQL Server têm suporte com o AD FS instalado com o Windows Server 2012:

-   SQL Server 2008 \/ R2

-   SQL Server 2012

## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento do servidor
Semelhante ao farm de servidores de Federação com a topologia WID, todos os servidores de Federação no farm são configurados para usar um nome DNS do sistema de nome de domínio de cluster \( \) \( que representa o nome de serviço de Federação \) e um endereço IP de cluster como parte da configuração de cluster NLB de balanceamento de carga de rede \( \) . Isso ajuda o host NLB a alocar solicitações de cliente para os servidores de Federação individuais. Os proxies de servidor de Federação podem ser usados para fazer proxy de solicitações de cliente para o farm de servidores de Federação.

A ilustração a seguir mostra como a empresa fictícia Contoso farmacêuticas implantou seu farm de servidores de Federação com SQL Server topologia na rede corporativa. Ele também mostra como essa empresa configurou a rede de perímetro com acesso a um servidor DNS, um host NLB adicional que usa o mesmo nome DNS de cluster \( FS.contoso.com \) que é usado no cluster NLB de rede corporativa e com dois proxies de servidor de Federação \( fsp1 e fsp2 \) .

![farm de servidores usando SQL](media/FarmSQLProxies.gif)

Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies de servidor de Federação, consulte [requisitos de resolução de nomes para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) ou [requisitos de resolução de nomes para proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
