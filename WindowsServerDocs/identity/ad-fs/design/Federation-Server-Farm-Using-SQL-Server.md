---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: Farm de servidores de federação usando SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 585d0195b096056ba769f4e9a08d5c4d2156b96a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191454"
---
# <a name="federation-server-farm-using-sql-server"></a>Farm de servidores de federação usando SQL Server

Essa topologia para os serviços de Federação do Active Directory \(do AD FS\) difere do farm de servidor de Federação usando o banco de dados interno do Windows \(WID\) topologia de implantação em que ele não replica os dados para cada servidor de federação no farm. Em vez disso, todos os servidores de federação no farm podem ler e gravar dados em um banco de dados comum que é armazenado em um servidor executando o Microsoft SQL Server que está localizado na rede corporativa.  
  
> [!IMPORTANT]  
> Se você quiser criar um farm do AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012 e SQL Server 2014.  
  
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
As seguintes versões do SQL server são compatíveis com o AD FS no Windows Server 2012 R2:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de posicionamento e a rede de servidor  
Semelhante ao farm de servidores de federação com topologia WID, todos os servidores de federação no farm estão configurados para usar o sistema de nomes de domínio de um cluster \(DNS\) nome \(que representa o nome do serviço de Federação\)e o endereço IP de um cluster como parte do balanceamento de carga de rede \(NLB\) configuração de cluster. Isso ajuda a host NLB alocar solicitações de cliente para os servidores de Federação individuais. Proxies de servidor de federação podem ser usado para solicitações de cliente de proxy para o farm de servidores de Federação.  
  
A ilustração a seguir mostra como a empresa fictícia Contoso Pharmaceuticals implantados seu farm de servidores de federação com topologia de SQL Server na rede corporativa. Ele também mostra como essa empresa configurou a rede de perímetro com acesso a um servidor DNS, um host NLB adicional que usa o mesmo nome DNS de cluster \(fs.contoso.com\) que é usado no cluster NLB de rede corporativa e com dois web proxies de aplicativo \(wap1 e wap2\).  
  
![farm de servidores usando o SQL](media/SQLFarmADFSBlue.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies do aplicativo web, consulte "Requisitos de resolução de nome" seção [requisitos do AD FS](AD-FS-Requirements.md) e [plano da Web Infraestrutura do Proxy de aplicativo (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Opções de alta disponibilidade para Farms de servidores SQL  
No Windows Server 2012 R2, o AD FS há são duas novas opções para oferecer suporte a alta disponibilidade em farms de servidores do AD FS usando o SQL Server.  
  
-   Suporte para grupos de disponibilidade do AlwaysOn do SQL Server  
  
-   Suporte para alta disponibilidade distribuído geograficamente, usando a replicação de mesclagem do SQL Server  
  
Esta seção descreve cada uma dessas opções, quais problemas eles solucionar, respectivamente e algumas considerações importantes para decidir quais opções para implantar.  
  
> [!NOTE]  
> Farms do AD FS que usam o banco de dados interno do Windows \(WID\) fornecer redundância de dados básicos com leitura\/acesso de gravação no nó do servidor de Federação primário e leitura\-só acessar em nós secundários.  Isso pode ser usado em um local geograficamente ou em uma topologia distribuída geograficamente.  
>   
> Ao usar o WID esteja ciente das seguintes limitações:  
>   
> -   Um farm de WID tem um limite de 30 servidores de federação, se você tiver 100 ou menos objetos de confiança.  
> -   Um farm de WID não oferece suporte a resolução de artefato ou de detecção de reprodução de token \(faz parte do Security Assertion Markup Language \(SAML\) protocolo\).  
  
A tabela a seguir fornece um resumo para o uso de um farm de WID.  
  
||||  
|-|-|-|  
||1 \- 100 relações de confiança RP|Relações de confiança RP mais de 100|  
|1 \- 30 AD FS nós|Suportada de WID|Não há suportada usando o WID \- SQL é necessário|  
|Mais de 30 AD FS nós|Não há suportada usando o WID \- SQL é necessário|Não há suportada usando o WID \- SQL é necessário|  
  
### <a name="alwayson-availability-groups"></a>Grupos de disponibilidade AlwaysOn  
**Visão geral**  
  
Grupos de disponibilidade AlwaysOn introduzidos no SQL Server 2012 e fornecem uma nova maneira de criar uma instância do SQL Server de alta disponibilidade.  Grupos de disponibilidade AlwaysOn combinam elementos de clustering e espelhamento de banco de dados para redundância e failover na camada de instância de SQL e a camada de banco de dados.  Ao contrário das opções anteriores de alta disponibilidade, grupos de disponibilidade AlwaysOn não exigem um armazenamento comum \(ou de rede de área de armazenamento\) na camada de banco de dados.  
  
Um grupo de disponibilidade é composto de uma réplica primária \(um conjunto de leitura\-gravar bancos de dados primários\) e uma a quatro réplicas de disponibilidade \(os conjuntos de bancos de dados secundários correspondentes\).  O grupo de disponibilidade dá suporte a uma única leitura\-gravar cópia \(a réplica primária\), e uma a quatro leitura\-somente as réplicas de disponibilidade.  Cada réplica de disponibilidade deve residir em um nó diferente de um único Windows Server Failover Clustering \(WSFC\) cluster.  Para obter mais informações sobre a disponibilidade do AlwaysOn grupos, consulte [visão geral dos grupos de disponibilidade AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Da perspectiva de nós de um farm do SQL Server do AD FS, o grupo de disponibilidade do AlwaysOn substitui a instância única do SQL Server como a diretiva \/ banco de dados de artefato.  O ouvinte do grupo de disponibilidade é que o cliente \(o serviço de token de segurança do AD FS\) usa para se conectar ao SQL.  
  
O diagrama a seguir mostra um Farm de servidores do AD FS SQL com o grupo de disponibilidade AlwaysOn.  
  
![farm de servidores usando o SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Grupos de disponibilidade AlwaysOn exigem que instâncias do SQL Server residem no Windows Server Failover Clustering \(WSFC\) nós.  
  
> [!NOTE]  
> Apenas uma réplica de disponibilidade pode agir como um destino de failover automático, o outro três se baseia em failovers manuais.  
  
**Principais considerações de implantação**  
  
Se você planeja usar grupos de disponibilidade AlwaysOn em combinação com a replicação de mesclagem do SQL Server, por favor, anote os problemas descritos em "Considerações de implantação de chave para uso do AD FS com a replicação de mesclagem do SQL Server" abaixo.  Em particular, quando um grupo de disponibilidade AlwaysOn contendo um banco de dados que é um assinante de replicação executar failover, a assinatura de replicação falha. Para continuar a replicação, um administrador de replicação deve reconfigurar o assinante manualmente.  Consulte a descrição do SQL Server de um problema específico no [assinantes de replicação e grupos de disponibilidade AlwaysOn \(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx) e suporte a instruções gerais para grupos de disponibilidade do AlwaysOn com Opções de replicação no [replicação, controle de alterações, Change Data Capture e grupos de disponibilidade AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
**Configurando o AD FS para usar um grupo de disponibilidade do AlwaysOn**  
  
A configuração de um farm do AD FS com grupos de disponibilidade AlwaysOn requer uma leve modificação para o procedimento de implantação do AD FS:  
  
1.  Os bancos de dados que você deseja fazer backup devem ser criados antes que os grupos de disponibilidade do AlwaysOn podem ser configurados.  O AD FS cria seus bancos de dados como parte da instalação e configuração inicial do primeiro nó de serviço de federação de um novo farm do SQL Server do AD FS.  Como parte da configuração do AD FS, você deve especificar uma cadeia de caracteres de conexão do SQL, portanto, você terá que configurar o primeiro nó do farm do AD FS para se conectar diretamente a uma instância do SQL \(isso é apenas temporário\).   Para obter orientações específicas sobre como configurar um farm do AD FS, incluindo a configuração de um nó do farm do AD FS com uma cadeia de conexão do SQL server, consulte [configurar um servidor de Federação](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Depois que os bancos de dados do AD FS tiveram sido criados, atribuí-los a grupos de disponibilidade AlwaysOn e criar o ouvinte de TCP/IP comuns usando ferramentas do SQL Server e processar cada [criação e configuração dos grupos de disponibilidade \(doSQLServer\) ](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Por fim, use o PowerShell para editar as propriedades do AD FS para atualizar a cadeia de conexão SQL para usar o endereço DNS do ouvinte do grupo de disponibilidade do AlwaysOn.  
  
    Comandos de PSH de exemplo para atualizar a cadeia de conexão do SQL para o banco de dados de configuração do AD FS:  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  Comandos de PSH de exemplo para atualizar a cadeia de conexão do SQL para o banco de dados de serviço de resolução de artefato de AD FS:  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>Replicação de mesclagem do SQL Server  
A replicação de mesclagem também introduzido no SQL Server 2012, permite a redundância de dados de política do AD FS com as seguintes características:  
  
-   Ler e gravar o recurso em todos os nós \(não apenas o primário\)  
  
-   Pequenas quantidades de dados replicados de forma assíncrona para evitar a introdução de latência para o sistema  
  
O diagrama a seguir mostra um farms de servidores do SQL Server do AD FS com redundância geográfica com replicação de mesclagem \(1 publicador, 2 assinantes\):  
  
![farm de servidores usando o SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Considerações de implantação de chave para usar o AD FS com replicação de mesclagem do SQL Server \(Observe os números no diagrama acima\)**  
  
-   Não há suporte para o banco de dados do distribuidor para uso com grupos de disponibilidade AlwaysOn ou espelhamento de banco de dados.  Consulte SQL Server oferece suporte a instruções para grupos de disponibilidade do AlwaysOn com as opções de replicação no [replicação, controle de alterações, Change Data Capture e grupos de disponibilidade AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
-   Quando um grupo de disponibilidade AlwaysOn contendo um banco de dados que é um assinante de replicação executar failover, a assinatura de replicação falha. Para continuar a replicação, um administrador de replicação deve reconfigurar o assinante manualmente.  Consulte a descrição do SQL Server de um problema específico no [assinantes de replicação e grupos de disponibilidade AlwaysOn \(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx) e suporte a instruções gerais para grupos de disponibilidade do AlwaysOn com Opções de replicação [replicação, controle de alterações, Change Data Capture e grupos de disponibilidade AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
Para obter instruções mais detalhadas sobre como configurar o AD FS para usar uma replicação de mesclagem do SQL Server, consulte [instalação de redundância geográfica com replicação do SQL Server](https://technet.microsoft.com/library/dn632406.aspx).  
  
## <a name="see-also"></a>Consulte também  
[Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guia de design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

