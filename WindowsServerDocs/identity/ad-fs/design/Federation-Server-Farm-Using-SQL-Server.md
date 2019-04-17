---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: "Farm de servidores de Federação usando o SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2333f79c733415833b1d54afc8c385700ac5581e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>Farm de servidores de Federação usando o SQL Server

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Essa topologia para serviços de Federação do Active Directory \(AD FS\) difere farm de servidor de Federação usando topologia de implantação do banco de dados interno do Windows \(WID\) que ele não replica os dados para cada servidor de federação no farm. Em vez disso, todos os servidores de federação no farm podem ler e gravar dados em um banco de dados comum que é armazenado em um servidor que executa o Microsoft SQL Server que está localizado na rede corporativa.  
  
> [!IMPORTANT]  
> Se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e o SQL Server 2014.  
  
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
As seguintes versões do servidor SQL são compatíveis com o AD FS no Windows Server 2012 R2:  
  
-   SQL Server 2008 \ / R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento de servidor  
Assim como o farm de servidores de federação com topologia de trabalho, todos os servidores de federação no farm estão configurados para usar um nome do cluster \(DNS\) Domain Name System \ (que representa o serviço de Federação name\) e o endereço IP de um cluster como parte da configuração do cluster \(NLB\) balanceamento de carga de rede. Isso ajuda o host NLB alocar solicitações de cliente para os servidores de Federação individuais. Proxies de servidor de federação podem ser usado para solicitações de cliente de proxy para o farm de servidores de Federação.  
  
A ilustração a seguir mostra como a empresa fictícia Contoso produtos farmacêuticos implantado seu farm de servidores de federação com topologia do SQL Server na rede corporativa. Ele também mostra como essa empresa configurado a rede do perímetro com acesso a um servidor DNS, um host NLB adicional que usa o mesmo cluster DNS nome \(fs.contoso.com\) que é usado no cluster NLB rede corporativa, e com dois proxies de aplicativo web \(wap1 and wap2\).  
  
![Farm de servidores usando o SQL](media/SQLFarmADFSBlue.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies de aplicativo da web, consulte "Requisitos de resolução de nome" seção [AD FS requisitos](AD-FS-Requirements.md) e [planejar a infraestrutura de Proxy de aplicativos Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Opções de alta disponibilidade para SQL Server Farms  
No Windows Server 2012 R2, o AD FS lá são duas novas opções para dar suporte a alta disponibilidade em farms AD FS usando o SQL Server.  
  
-   Suporte para grupos de disponibilidade do SQL Server AlwaysOn  
  
-   Suporte para alta disponibilidade geograficamente distribuído, usando a replicação de mesclagem do SQL Server  
  
Esta seção descreve cada uma dessas opções, quais problemas eles resolvem respectivamente e algumas considerações importantes para decidir quais opções para implantar.  
  
> [!NOTE]  
> Farms de FS anúncios que usam \(WID\) banco de dados interno do Windows fornecem redundância dados básicos com acesso read\/gravação no nó do servidor de Federação principal e acesso somente read\ em nós secundários.  Isso pode ser usado em um local geograficamente ou uma topologia geograficamente distribuída.  
>   
> Ao usar o trabalho esteja ciente das seguintes limitações:  
>   
> -   Um farm de trabalho tem um limite de 30 servidores de federação se você tiver 100 ou menos terceira parte relações de confiança.  
> -   Um farm de trabalho não oferece suporte a resolução de detecção ou artefato de repetição token \ (parte do \(SAML\) protocol\ a segurança asserção Markup Language).  
  
A tabela a seguir fornece um resumo para usar um farm de trabalho.  
  
||||  
|-|-|-|  
||1 \-100 relações de confiança RP|Mais de 100 relações de confiança RP|  
|1 \-30 AD FS nós|Trabalho com suporte|Não permitido usando trabalho \-SQL necessária|  
|Mais de 30 AD FS nós|Não permitido usando trabalho \-SQL necessária|Não permitido usando trabalho \-SQL necessária|  
  
### <a name="alwayson-availability-groups"></a>Grupos de disponibilidade do AlwaysOn  
**Visão geral**  
  
Disponibilidade do AlwaysOn grupos foram introduzidas no SQL Server 2012 e fornece uma nova maneira de criar uma instância do SQL Server de alta disponibilidade.  Grupos de disponibilidade do AlwaysOn combinam elementos de clustering e espelhamento de banco de dados para redundância e failover a camada de instância do SQL e a camada de banco de dados.  Ao contrário de opções de alta disponibilidade anteriores, grupos de disponibilidade do AlwaysOn não exigem um armazenamento comuns \ (ou rede \ de área de armazenamento) na camada de banco de dados.  
  
Um grupo de disponibilidade é composto de uma réplica principal \ (um conjunto de gravação read\ principal databases\) réplicas de disponibilidade e um a quatro \ (conjuntos de databases\ secundário correspondente).  O grupo de disponibilidade dá suporte a uma cópia de gravação de read\ único \ (replica\ primário), e um a quatro réplicas de disponibilidade somente read\.  Cada réplica disponibilidade deve residir em um nó diferente de um único cluster \(WSFC\) cluster de Failover do Windows Server.  Para obter mais informações sobre disponibilidade AlwaysOn grupos, consulte [visão geral dos grupos de disponibilidade do AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Da perspectiva de nós de um farm AD FS SQL Server, o grupo de disponibilidade do AlwaysOn substitui a única instância do SQL Server como a política \ / banco de dados de artefato.  O ouvinte do grupo de disponibilidade é que o cliente \ (o AD FS segurança token intelegente) usa para se conectar a SQL.  
  
O diagrama a seguir mostra um Farm de servidores do AD FS SQL com o grupo de disponibilidade do AlwaysOn.  
  
![Farm de servidores usando o SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Grupos de disponibilidade do AlwaysOn exigem que as instâncias do SQL Server residam em nós de cluster de Failover do Windows Server \(WSFC\).  
  
> [!NOTE]  
> Apenas uma réplica de disponibilidade pode atuar como um destino de failover automático, os outros três contará com failovers manual.  
  
**Considerações sobre a implantação de chave**  
  
Se você pretende usar grupos de disponibilidade do AlwaysOn em combinação com a replicação de mesclagem do SQL Server, por favor, anote os problemas descritos em "Considerações de implantação de chave para usar o AD FS com replicação de mesclagem do SQL Server" abaixo.  Em particular, quando um grupo de disponibilidade do AlwaysOn que contenha um banco de dados que seja um assinante de replicação faz failover, a assinatura de replicação falha. Para retomar a replicação, um administrador de replicação deve reconfigurar manualmente o assinante.  Veja a descrição do SQL Server do problema específico na [\(SQL Server\) assinantes de replicação e grupos de disponibilidade do AlwaysOn](https://technet.microsoft.com/library/hh882436.aspx) e suporte geral instruções para grupos de disponibilidade do AlwaysOn com opções de replicação em [\(SQL Server\) replicação, controle de alterações, captura de dados alterados e grupos de disponibilidade do AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
**Configurando o AD FS para usar um grupo de disponibilidade do AlwaysOn**  
  
Configurar um farm AD FS com grupos de disponibilidade do AlwaysOn requer uma leve modificação o procedimento de implantação do AD FS:  
  
1.  Os bancos de dados que você deseja fazer backup devem ser criados antes que os grupos de disponibilidade do AlwaysOn podem ser configurados.  AD FS cria seus bancos de dados como parte da instalação e configuração inicial do primeiro nó de serviço de federação de um novo farm AD FS SQL Server.  Como parte da configuração do AD FS, você deve especificar uma cadeia de caracteres de conexão do SQL, portanto, você terá que configurar o primeiro nó farm AD FS para se conectar diretamente a uma instância do SQL \ (Isso é apenas temporary\).   Para obter diretrizes específicas sobre como configurar um farm AD FS, inclusive como configurar um nó de farm AD FS com uma cadeia de conexão do SQL server, consulte [configurar um servidor de Federação](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Depois que os bancos de dados do AD FS tiverem sido criados, atribuí-las a grupos de disponibilidade do AlwaysOn e crie o ouvinte TCPIP comuns usando as ferramentas de SQL Server e processar por [\(SQL Server\) criação e configuração dos grupos de disponibilidade](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Por fim, use o PowerShell para editar as propriedades do AD FS para atualizar a cadeia de caracteres de conexão SQL para usar o endereço de ouvinte do grupo de disponibilidade do AlwaysOn DNS.  
  
    Comandos PSH de exemplo para atualizar a cadeia de caracteres de conexão do SQL para o banco de dados do AD FS política:  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  Comandos PSH de exemplo para atualizar a cadeia de caracteres de conexão do SQL para o banco de dados do AD FS política:  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>Replicação de mesclagem do SQL Server  
Também foi introduzido no SQL Server 2012, a replicação de mesclagem permite que o AD FS política redundância de dados com as seguintes características:  
  
-   Ler e gravar a funcionalidade em todos os nós \ (e não apenas o primary\)  
  
-   Quantidades menores de dados replicados assincronamente para evitar a introdução de latência ao sistema  
  
O diagrama a seguir mostra um geograficamente redundante farms AD FS SQL Server com replicação de mesclagem \ (1 fornecedor, 2 subscribers\):  
  
![Farm de servidores usando o SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Considerações de implantação de chave para usar o AD FS com replicação de mesclagem do SQL Server \ (Observe os números no above\ diagrama)**  
  
-   Não há suporte para o banco de dados de distribuidor para uso com grupos de disponibilidade do AlwaysOn ou espelhamento de banco de dados.  Consulte SQL Server suporte a instruções para grupos de disponibilidade do AlwaysOn com opções de replicação em [\(SQL Server\) replicação, controle de alterações, captura de dados alterados e grupos de disponibilidade do AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
-   Quando um grupo de disponibilidade do AlwaysOn que contenha um banco de dados que seja um assinante de replicação faz failover, a assinatura de replicação falha. Para retomar a replicação, um administrador de replicação deve reconfigurar manualmente o assinante.  Veja a descrição do SQL Server do problema específico na [\(SQL Server\) assinantes de replicação e grupos de disponibilidade do AlwaysOn](https://technet.microsoft.com/library/hh882436.aspx) e suporte geral instruções para grupos de disponibilidade do AlwaysOn com opções de replicação [\(SQL Server\) replicação, controle de alterações, captura de dados alterados e grupos de disponibilidade do AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
Para obter instruções mais detalhadas sobre como configurar o AD FS usar uma replicação de mesclagem do SQL Server, consulte [instalação redundância geográfica com replicação do SQL Server](https://technet.microsoft.com/en-us/library/dn632406.aspx).  
  
## <a name="see-also"></a>Consulte também  
[Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guia de Design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

