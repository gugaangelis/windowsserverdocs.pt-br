---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: Farm de servidores de federação usando SQL Server
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fd82edbcd2403416a08a4d707e5271ab2ced4b22
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853119"
---
# <a name="federation-server-farm-using-sql-server"></a>Farm de servidores de federação usando SQL Server

Essa topologia para Serviços de Federação do Active Directory (AD FS) \(AD FS\) difere do farm de servidores de Federação usando o banco de dados interno do Windows \(o WID\) a topologia de implantação, pois ele não replica o dado para cada servidor de Federação no farm. Em vez disso, todos os servidores de Federação no farm podem ler e gravar dados em um banco de dado comum que é armazenado em um servidor que executa Microsoft SQL Server que está localizado na rede corporativa.  
  
> [!IMPORTANT]  
> Se você quiser criar um farm de AD FS e usar SQL Server para armazenar seus dados de configuração, poderá usar SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e SQL Server 2014.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve as várias considerações sobre o público-alvo, os benefícios e as limitações associados a essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   Grandes organizações com mais de 100 relações de confiança que precisam fornecer a usuários internos e usuários externos com\-de logon único em \(SSO\) acesso a aplicativos ou serviços federados  
  
-   Organizações que já usam SQL Server e desejam aproveitar suas ferramentas e especialistas existentes  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Suporte para números maiores de relações de confiança \(mais de 100\)  
  
-   O suporte para detecção de reprodução de token \(um recurso de segurança\) e resolução de artefato \(parte do Security Assertion Markup Language \(protocolo SAML\) 2,0\)  
  
-   Suporte para todos os benefícios do SQL Server, como espelhamento de banco de dados, clustering de failover, relatórios e ferramentas de gerenciamento  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   Essa topologia não fornece redundância de banco de dados por padrão. Embora um farm de servidores de Federação com topologia de WID replique automaticamente o banco de dados WID em cada servidor de Federação no farm, o farm de servidores de Federação com SQL Server topologia contém apenas uma cópia do banco de dados  
  
> [!NOTE]  
> O SQL Server dá suporte a muitos dados e opções de redundância de aplicativo diferentes, incluindo clustering de failover, espelhamento de banco de dado e vários tipos diferentes de replicação de SQL Server.  
  
A tecnologia de informações da Microsoft \(ti\) departamento usa SQL Server o espelhamento de banco de dados no High\-Safety \(modo de\) síncrono e clustering de failover para fornecer suporte de alta disponibilidade\-para a instância de SQL Server. SQL Server \(de ponto transacional\-para\-ponto de\) e replicação de mesclagem não foram testados pela equipe de produto do AD FS na Microsoft. Para obter mais informações sobre SQL Server, consulte [visão geral das soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853) ou [selecionando o tipo apropriado de replicação](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versões SQL Server com suporte  
As seguintes versões do SQL Server têm suporte com o AD FS no Windows Server 2012 R2:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento do servidor  
Semelhante ao farm de servidores de Federação com a topologia de WID, todos os servidores de Federação no farm são configurados para usar um sistema de nome de domínio de cluster \(nome de\) DNS \(que representa o nome de Serviço de Federação\) e um endereço IP de cluster como parte do balanceamento de carga de rede \(configuração de cluster do NLB\). Isso ajuda o host NLB a alocar solicitações de cliente para os servidores de Federação individuais. Os proxies de servidor de Federação podem ser usados para fazer proxy de solicitações de cliente para o farm de servidores de Federação.  
  
A ilustração a seguir mostra como a empresa fictícia Contoso farmacêuticas implantou seu farm de servidores de Federação com SQL Server topologia na rede corporativa. Ele também mostra como essa empresa configurou a rede de perímetro com acesso a um servidor DNS, um host NLB adicional que usa o mesmo nome DNS de cluster \(fs.contoso.com\) que é usado no cluster NLB de rede corporativa e com dois proxies de aplicativo Web \(wap1 e WAP2\).  
  
![farm de servidores usando SQL](media/SQLFarmADFSBlue.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies de aplicativo Web, consulte a seção "requisitos de resolução de nomes" em [AD FS requisitos](AD-FS-Requirements.md) e [planejar a WAP (infraestrutura de proxy de aplicativo Web)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Opções de alta disponibilidade para farms de SQL Server  
No Windows Server 2012 R2, AD FS há duas novas opções para dar suporte à alta disponibilidade em farms de AD FS usando o SQL Server.  
  
-   Suporte para SQL Server Grupos de Disponibilidade AlwaysOn  
  
-   Suporte para alta disponibilidade distribuída geograficamente usando replicação de mesclagem SQL Server  
  
Esta seção descreve cada uma dessas opções, quais problemas eles resolvem respectivamente e algumas considerações importantes para decidir quais opções implantar.  
  
> [!NOTE]  
> Os farms de AD FS que usam o banco de dados interno do Windows \(WID\) fornecem redundância de dado básica com acesso de leitura\/gravação no nó do servidor de Federação primário e leitura\-somente acesso em nós secundários.  Isso pode ser usado em uma topologia geograficamente local ou distribuída geograficamente.  
>   
> Ao usar o WID, lembre-se das seguintes limitações:  
>   
> -   Um farm WID tem um limite de 30 servidores de Federação se você tiver 100 ou menos confianças de terceira parte confiável.  
> -   Um farm WID não dá suporte à detecção de reprodução de token ou à resolução de artefatos \(parte do Security Assertion Markup Language \(protocolo\) SAML\).  
  
A tabela a seguir fornece um resumo para usar um farm WID.  
  
||||  
|-|-|-|  
||1 \- de confianças do RP 100|Mais de 100 RP relações de confiança|  
|1 \- 30 nós AD FS|Suporte ao WID|Sem suporte usando o WID \- SQL necessário|  
|Mais de 30 nós AD FS|Sem suporte usando o WID \- SQL necessário|Sem suporte usando o WID \- SQL necessário|  
  
### <a name="alwayson-availability-groups"></a>Grupos de disponibilidade AlwaysOn  
**Visão geral**  
  
Os grupos de disponibilidade AlwaysOn foram introduzidos no SQL Server 2012 e fornecem uma nova maneira de criar uma instância de SQL Server de alta disponibilidade.  Os grupos de disponibilidade AlwaysOn combinam elementos de clustering e espelhamento de banco de dados para redundância e failover na camada de instância do SQL e na camada de banco de dados.  Ao contrário das opções de alta disponibilidade anteriores, os grupos de disponibilidade AlwaysOn não exigem um \(de armazenamento comum nem uma rede de área de armazenamento\) na camada de banco de dados.  
  
Um grupo de disponibilidade é composto de uma réplica primária \(um conjunto de bancos de dados primários de leitura\-gravação\) e de uma a quatro réplicas de disponibilidade \(conjuntos de bancos de dados secundários correspondentes\).  O grupo de disponibilidade dá suporte a uma única cópia\-gravação \(a réplica primária\)e uma a quatro somente leitura\-réplicas de disponibilidade.  Cada réplica de disponibilidade deve residir em um nó diferente de um único clustering de failover do Windows Server \(cluster\) WSFC.  Para obter mais informações sobre grupos de disponibilidade AlwaysOn, consulte [visão geral do Grupos de Disponibilidade AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Da perspectiva dos nós de um farm de AD FS SQL Server, o grupo de disponibilidade AlwaysOn substitui a instância de SQL Server única como o banco de dados de \/ artefato de política.  O ouvinte do grupo de disponibilidade é o que o cliente \(o AD FS serviço de token de segurança do\) usa para se conectar ao SQL.  
  
O diagrama a seguir mostra um farm AD FS SQL Server com o grupo de disponibilidade AlwaysOn.  
  
![farm de servidores usando SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Os grupos de disponibilidade AlwaysOn exigem que as instâncias de SQL Server residam no clustering de failover do Windows Server \(nós de\) WSFC.  
  
> [!NOTE]  
> Somente uma réplica de disponibilidade pode atuar como um destino de failover automático, as outras três dependem de failovers manuais.  
  
**Principais considerações sobre implantação**  
  
Se você planeja usar grupos de disponibilidade AlwaysOn em combinação com SQL Server replicação de mesclagem, anote os problemas descritos em "principais considerações de implantação para usar AD FS com replicação de mesclagem SQL Server" abaixo.  Em particular, quando um grupo de disponibilidade AlwaysOn contendo um banco de dados que é um assinante de replicação falha, a assinatura de replicação falha. Para retomar a replicação, um administrador de replicação deve reconfigurar o assinante manualmente.  Confira a descrição SQL Server de um problema específico em [assinantes de replicação e Grupos de Disponibilidade AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx) e instruções de suporte geral para grupos de disponibilidade AlwaysOn com opções de replicação em [replicação, controle de alterações, captura de dados de alterações e grupos de disponibilidade AlwaysOn ](https://technet.microsoft.com/library/hh403414.aspx)\(SQL Server\).  
  
**Configurando AD FS para usar um grupo de disponibilidade AlwaysOn**  
  
A configuração de um farm de AD FS com grupos de disponibilidade AlwaysOn requer uma ligeira modificação no procedimento de implantação de AD FS:  
  
1.  Os bancos de dados que você deseja fazer backup devem ser criados antes que os grupos de disponibilidade AlwaysOn possam ser configurados.  AD FS cria seus bancos de dados como parte da instalação e configuração inicial do primeiro nó de serviço de Federação de um novo farm de SQL Server de AD FS.  Como parte da configuração de AD FS, você deve especificar uma cadeia de conexão SQL, portanto, você precisará configurar o primeiro nó de farm AD FS para se conectar a uma instância do SQL diretamente \(isso é apenas um\)temporário.   Para obter diretrizes específicas sobre como configurar um farm de AD FS, incluindo a configuração de um nó de farm de AD FS com uma cadeia de conexão do SQL Server, consulte [configurar um servidor de Federação](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Depois que os bancos de dados do AD FS tiverem sido criados, atribua-os aos grupos de disponibilidade AlwaysOn e crie o ouvinte de TCPIP comum usando ferramentas de SQL Server e processo na [criação e configuração de grupos de disponibilidade \(SQL Server\)](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Por fim, use o PowerShell para editar as propriedades de AD FS para atualizar a cadeia de conexão SQL para usar o endereço DNS do ouvinte do grupo de disponibilidade AlwaysOn.  
  
    Exemplo de comandos PSH para atualizar a cadeia de conexão SQL para o banco de dados de configuração de AD FS:  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring="data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true"  
    PS:\>$temp.put()  
  
    ```  
  
4.  Exemplo de comandos PSH para atualizar a cadeia de conexão SQL para o banco de dados do serviço de resolução de artefatos AD FS:  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection "Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True"  
    ```  
  
### <a name="sql-server-merge-replication"></a>Replicação de mesclagem SQL Server  
Também introduzido no SQL Server 2012, a replicação de mesclagem permite a redundância de dados de AD FS política com as seguintes características:  
  
-   Capacidade de leitura e gravação em todos os nós \(não apenas os\) primários  
  
-   Quantidades menores de dados replicadas assincronamente para evitar a introdução da latência ao sistema  
  
O diagrama a seguir mostra um AD FS geograficamente redundante SQL Server farms com replicação de mesclagem \(1 Publicador, 2 assinantes\):  
  
![farm de servidores usando SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Principais considerações de implantação para usar AD FS com SQL Server replicação de mesclagem \(números de anotação no diagrama acima\)**  
  
-   O banco de dados distribuidor não tem suporte para uso com Grupos de Disponibilidade AlwaysOn ou espelhamento de banco de dados.  Consulte SQL Server instruções de suporte para grupos de disponibilidade AlwaysOn com opções de replicação em [replicação, controle de alterações, Change Data Capture e Grupos de Disponibilidade AlwaysOn \(SQL Server ](https://technet.microsoft.com/library/hh403414.aspx)\).  
  
-   Quando um grupo de disponibilidade AlwaysOn contendo um banco de dados que é um assinante de replicação executar failover, a assinatura de replicação apresentará falha. Para retomar a replicação, um administrador de replicação deve reconfigurar o assinante manualmente.  Confira a descrição SQL Server de um problema específico em [assinantes de replicação e Grupos de Disponibilidade AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx) e instruções de suporte geral para grupos de disponibilidade AlwaysOn com opções de replicação [replicação, controle de alterações, captura de dados de alterações e grupos de disponibilidade AlwaysOn ](https://technet.microsoft.com/library/hh403414.aspx)\(SQL Server\).  
  
Para obter instruções mais detalhadas sobre como configurar AD FS para usar uma replicação de mesclagem SQL Server, consulte [Configurar a redundância geográfica com replicação do SQL Server](https://technet.microsoft.com/library/dn632406.aspx).  
  
## <a name="see-also"></a>Consulte também  
[Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guia de design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

