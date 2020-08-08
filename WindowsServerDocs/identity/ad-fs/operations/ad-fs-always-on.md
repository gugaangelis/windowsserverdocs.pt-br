---
title: Configurando uma implantação de AD FS com Grupos de Disponibilidade AlwaysOn
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/20/2020
ms.topic: article
ms.openlocfilehash: c306f901aba2991a238fb994117789d4a9a81a67
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967123"
---
# <a name="setting-up-an-ad-fs-deployment-with-alwayson-availability-groups"></a>Configurando uma implantação de AD FS com Grupos de Disponibilidade AlwaysOn
Uma topologia distribuída geograficamente altamente disponível fornece:
* Eliminação de um único ponto de falha: com recursos de failover, você pode obter uma infraestrutura do ADFS altamente disponível, mesmo que um dos data centers de uma parte de um globo fique inativo.
* Desempenho aprimorado: você pode usar a implantação sugerida para fornecer uma infraestrutura de ADFS de alto desempenho

AD FS pode ser configurado para um cenário distribuído geograficamente altamente disponível.
O guia a seguir apresentará uma visão geral de AD FS com os grupos de disponibilidade AlwaysOn do SQL e fornecerá orientações e considerações de implantação.

## <a name="overview---alwayson-availability-groups"></a>Visão geral-Grupos de Disponibilidade AlwaysOn

Para obter mais informações sobre grupos de disponibilidade AlwaysOn, consulte [visão geral de grupos de disponibilidade AlwaysOn (SQL Server)](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-ver15)

Da perspectiva dos nós de um farm de AD FS SQL Server, o grupo de disponibilidade AlwaysOn substitui a instância de SQL Server única como banco de dados de política/artefato.O ouvinte do grupo de disponibilidade é o que o cliente (o AD FS serviço de token de segurança) usa para se conectar ao SQL.
O diagrama a seguir mostra um farm AD FS SQL Server com o grupo de disponibilidade AlwaysOn.

![farm de servidores usando SQL](media/ad-fs-always-on/SQLoverview.png)

Um AG (grupo de disponibilidade Always On) é um ou mais bancos de dados de usuário que fazem failover juntos. Um grupo de disponibilidade consiste em uma réplica de disponibilidade primária e uma a quatro réplicas secundárias que são mantidas por SQL Server movimentação de dados baseada em log para proteção de dados sem a necessidade de armazenamento compartilhado. Cada réplica é hospedada por uma instância do SQL Server em um nó diferente do WSFC. O grupo de disponibilidade e um nome de rede virtual correspondente são registrados como recursos no cluster do WSFC.

Um ouvinte de grupo de disponibilidade no nó da réplica primária responde às solicitações de entrada do cliente para se conectar ao nome da rede virtual e, com base nos atributos da cadeia de conexão, redireciona cada solicitação para a instância de SQL Server apropriada.
No caso de um failover, em vez de transferir a propriedade de recursos físicos compartilhados para outro nó, o WSFC é utilizado para reconfigurar uma réplica secundária em outra instância de SQL Server para se tornar a réplica primária do grupo de disponibilidade. O recurso de nome de rede virtual do grupo de disponibilidade é transferido para aquela instância.
A qualquer momento, apenas uma única instância de SQL Server pode hospedar a réplica primária dos bancos de dados de um grupo de disponibilidade, todas as réplicas secundárias associadas devem residir em uma instância separada, e cada instância deve residir em nós físicos separados.

> [!NOTE]
> Se os computadores estiverem em execução no Azure, configure as máquinas virtuais do Azure para permitir que a configuração do ouvinte se comunique com os grupos de disponibilidade AlwaysOn. Para obter mais informações, [máquinas virtuais: SQL Always on Listener](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener).

Para obter uma visão geral adicional do Grupos de Disponibilidade AlwaysOn, consulte [visão geral de Always on grupos de disponibilidade (SQL Server)](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-ver15).

> [!NOTE]
> Se a organização exigir failover em vários datacenters, é recomendável criar um banco de dados de artefato em cada datacenter, bem como habilitar um cache em segundo plano, o que reduz a latência durante o processamento da solicitação. Siga as instruções para fazer isso no [ajuste fino do SQL e na redução da latência](./adfs-sql-latency.md).

## <a name="deployment-guidance"></a>Diretrizes de implantação

1. <b>Considere o banco de dados correto para as metas da implantação de AD FS.</b> O AD FS usa um banco de dados para armazenar a configuração e, em alguns casos, em um dado transacional relacionado ao Serviço de Federação. Você pode usar AD FS software para selecionar o banco de dados interno do Windows (WID) ou o Microsoft SQL Server 2008 ou mais recente para armazenar o dado no serviço de Federação.
A tabela a seguir descreve as diferenças nos recursos com suporte entre um WID e um banco de dados SQL.


| Categoria      | Recurso       | Com suporte pelo WID  | Com suporte do SQL |
| ------------------ |:-------------:| :---:|:---: |
| Recursos de AD FS     | Implantação do farm do servidor de federação | Sim  | Sim |
| Recursos de AD FS     | Resolução do artefato SAML. Observação: isso não é comum para aplicativos SAML     |   Não | Sim  |
| Recursos de AD FS | Detecção de reprodução de token SAML/WS-Federation. Observação: somente necessário quando AD FS recebe tokens de IDPs externos. Isso não será necessário se AD FS não estiver agindo como um parceiro de Federação.      |    Não  | Sim |
| Recursos de banco de dados     |   Redundância básica de banco de dados usando replicação pull, em que um ou mais servidores que hospedam uma cópia somente leitura das alterações de solicitação de banco de dados feitas em um host de servidor de origem uma cópia de leitura/gravação do banco de dados    |   Não | Não  |
| Recursos de banco de dados | Redundância de banco de dados usando soluções de alta disponibilidade, como clustering ou espelhamento (na camada de banco de dados)      |    Não  | Sim |
| Recursos Adicionais | Cenário de fatores OAuth     |   Sim  | Sim |

Se você for uma grande organização com mais de 100 relações de confiança que precisam fornecer a usuários internos e usuários externos com acesso de logon único a aplicativos ou serviços de Federação, o SQL é a opção recomendada.

Se você for uma organização com 100 ou menos relações de confiança configuradas, o WID fornecerá redundância de dados e de serviço de Federação (em que cada servidor de Federação Replica as alterações para outros servidores de Federação no mesmo farm). O WID não dá suporte à detecção de reprodução de token ou à resolução de artefatos e tem um limite de 30 servidores de Federação.
Para obter mais informações sobre como planejar sua implantação, visite [aqui](../design/planning-your-deployment.md).

## <a name="sql-server-high-availability-solutions"></a>SQL Server soluções de alta disponibilidade
Se você estiver usando SQL Server como seu banco de dados de configuração do AD FS, poderá configurar a redundância geográfica para seu farm de AD FS usando a replicação SQL Server. A redundância geográfica replica dados entre dois sites distantes geograficamente para que os aplicativos possam mudar de um site para outro. Dessa forma, em caso de falha de um site, você ainda pode ter todos os dados de configuração disponíveis no segundo site. 
Se o SQL for o banco de dados apropriado para suas metas de implantação, prossiga com este guia de implantação.

Este guia apresentará o seguinte
* Implantar AD FS
* Configurar AD FS para usar um Grupos de Disponibilidade AlwaysOn
* Instalar a função de clustering de failover
* Executar testes de validação de cluster
* Habilitar grupos de disponibilidade de Always On
* Fazer backup de bancos de dados AD FS
* Criar grupos de disponibilidade AlwaysOn
* Adicionar bancos de dados no segundo nó
* Unir uma réplica de disponibilidade a um grupo de disponibilidade
* Atualizar a cadeia de conexão SQL

## <a name="deploy-ad-fs"></a>Implantar AD FS

> [!NOTE]
> Se os computadores estiverem em execução no Azure, as máquinas virtuais deverão ser configuradas de uma maneira específica para permitir que o ouvinte se comunique com o grupo de Always On disponibilidade. Para obter informações sobre a configuração, veja [configurar um balanceador de carga para um grupo de disponibilidade em VMs de SQL Server do Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener)


Este guia de implantação mostrará um farm de dois nós com dois SQL Servers como exemplo.
Para implantar AD FS siga os links iniciais abaixo para instalar o serviço de função AD FS. Para configurar o para um grupo AoA, haverá etapas adicionais para a função.
-   [Adicionar um computador a um domínio](../deployment/join-a-computer-to-a-domain.md)
-   [Registrar um certificado SSL do AD FS](../deployment/enroll-an-ssl-certificate-for-ad-fs.md)
-   [Instalar o serviço de função do AD FS](../deployment/install-the-ad-fs-role-service.md)


## <a name="configuring-ad-fs-to-use-an-alwayson-availability-group"></a>Configurando AD FS para usar um grupo de disponibilidade AlwaysOn

Configurar um farm de AD FS com grupos de disponibilidade AlwaysOn requer uma ligeira modificação no procedimento de implantação de AD FS. Certifique-se de que cada instância de servidor esteja executando a mesma versão do SQL. Para exibir a lista completa de pré-requisitos, restrições e recomendações para grupos de disponibilidade Always On, leia [aqui](/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability?view=sql-server-2017#PrerequisitesForDbs).

1.  Os bancos de dados que você deseja fazer backup devem ser criados antes que os grupos de disponibilidade AlwaysOn possam ser configurados.  AD FS cria seus bancos de dados como parte da instalação e configuração inicial do primeiro nó de serviço de Federação de um novo farm de SQL Server de AD FS.  Especifique o nome do host do banco de dados para o farm existente usando o SQL Server. Como parte da configuração de AD FS, você deve especificar uma cadeia de conexão SQL, portanto, você precisará configurar o primeiro farm de AD FS para se conectar a uma instância do SQL diretamente (isso é apenas temporário). Para obter diretrizes específicas sobre como configurar um farm de AD FS, incluindo a configuração de um nó de farm de AD FS com uma cadeia de conexão do SQL Server, consulte [configurar um servidor de Federação](../deployment/configure-a-federation-server.md).

![especificar farm](media/ad-fs-always-on/deploymentSpecifyFarm.png)

2.  Verifique a conectividade com o banco de dados usando o SSMS e conecte-se ao nome de host do banco de dados de destino. Se estiver adicionando outro nó ao farm de Federação, conecte-se ao banco de dados de destino.
3.  Especifique o certificado SSL para o farm de AD FS.

![especificar certificado SSL](media/ad-fs-always-on/deploymentSpecifySSL.png)

4.  Conecte o farm a uma conta de serviço ou gMSA.

![especificar conta de serviço](media/ad-fs-always-on/deploymentSpecifyServiceAccount.png)

5.  Conclua a configuração e a instalação do farm de AD FS.

> [!NOTE]
> SQL Server deve ser executado em uma conta de domínio para a instalação de Always On grupos de disponibilidade. Por padrão, ele é executado como um sistema local.

## <a name="install-the-failover-clustering-role"></a>Instalar a função de clustering de failover
A função de cluster de failover do Windows Server fornece o para obter mais informações sobre clusters de failover do Windows Server,
1.  Inicie o Gerenciador do Servidor.
2.  No menu gerenciar, selecione Adicionar funções e recursos.
3.  Na página antes de começar, selecione avançar.
4.  Na página Selecionar tipo de instalação, selecione instalação baseada em função ou recurso e, em seguida, selecione avançar.
5.  Na página Selecionar servidor de destino, selecione o SQL Server no qual você deseja instalar o recurso e, em seguida, selecione avançar.

![servidores de destino](media/ad-fs-always-on/clusteringDestinationServer.png)

6.  Na página Selecionar funções do servidor, selecione Avançar.
7.  Na página Selecionar recursos, marque a caixa de seleção Clustering de Failover.

![selecionar recurso de clustering](media/ad-fs-always-on/clusteringFeature.png)

8.  Na página confirmar seleções de instalação, selecione instalar.
A reinicialização do servidor não é necessária para o recurso de Clustering de Failover.
9.  Quando a instalação for concluída, selecione fechar.
10. Repita esse procedimento em todos os servidores que deseja adicionar como nós de cluster de failover.

## <a name="run-cluster-validation-tests"></a>Executar testes de validação de cluster
1.  Em um computador que tenha Ferramentas de Gerenciamento de Cluster de Failover instaladas a partir das Ferramentas de Administração de Servidor Remoto ou em um servidor no qual você tenha instalado o recurso de Clustering de Failover, inicie o Gerenciador de Cluster de Failover. Para fazer isso em um servidor, inicie o Gerenciador do Servidor e, no menu ferramentas, selecione Gerenciador de Cluster de Failover.
2.  No painel de Gerenciador de Cluster de Failover, em gerenciamento, selecione Validar configuração.
3.  Na página Antes de Começar escolha Avançar.
4.  Na página selecionar servidores ou um cluster, na caixa Inserir nome, insira o nome NetBIOS ou o nome de domínio totalmente qualificado de um servidor que você planeja adicionar como um nó de cluster de failover e, em seguida, selecione Adicionar. Repita essa etapa para cada servidor que quiser adicionar. Para adicionar vários servidores ao mesmo tempo, separe os nomes usando uma vírgula ou um ponto e vírgula. Por exemplo, digite os nomes no formato server1.contoso.com, server2.contoso.com. Quando tiver terminado, selecione Avançar.

![selecionar imagem de servidores](media/ad-fs-always-on/clusterValidationServers.png)

5. Na página opções de teste, selecione executar todos os testes (recomendado) e, em seguida, selecione avançar.
6. Na página confirmação, selecione avançar.
A página Validando exibe o status dos testes em execução.
7. Na página Resumo, realize uma destas ações:
- Se os resultados indicarem que os testes foram concluídos com êxito e a configuração for adequada para clustering e você quiser criar o cluster imediatamente, verifique se a caixa de seleção criar o cluster agora usando os nós validados está marcada e, em seguida, selecione concluir. Em seguida, continue na etapa 4 do [procedimento criar o cluster de failover](../../../failover-clustering/create-failover-cluster.md#create-the-failover-cluster).

![validar imagem de configuração](media/ad-fs-always-on/clusterValidationResults.png)

-   Se os resultados indicarem que houve avisos ou falhas, selecione Exibir relatório para exibir os detalhes e determinar quais problemas devem ser corrigidos. Observe que uma advertência para um teste de validação específico indica que esse aspecto do cluster de failover pode ser aceito, mas pode não atender às melhores práticas recomendadas.

> [!NOTE]
> Se você receber um aviso para o teste Validar reserva persistente para espaços de armazenamento, consulte a postagem no blog [Aviso de validação de cluster de failover do Windows indica que seus discos não dão suporte a reservas persistentes para espaços de armazenamento](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) para mais informações.
> Para obter mais informações sobre os testes de validação de hardware, consulte [Validate Hardware for a Failover Cluster (Validar o hardware de um cluster de failover)](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)).

## <a name="create-the-failover-cluster"></a>Criar o cluster de failover

Para concluir esta etapa, verifique se a conta de usuário com a qual você fez logon atende aos requisitos destacados na seção [Verificar os pré-requisitos](../../../failover-clustering/create-failover-cluster.md#verify-the-prerequisites) deste tópico.
1.  Inicie o Gerenciador do Servidor.
2.  No menu ferramentas, selecione Gerenciador de Cluster de Failover.
3.  No painel de Gerenciador de Cluster de Failover, em gerenciamento, selecione criar cluster.
O Assistente para Criação de Clusters é aberto.
4.  Na página Antes de Começar escolha Avançar.
5.  Se a página selecionar servidores aparecer, na caixa Inserir nome, insira o nome NetBIOS ou o nome de domínio totalmente qualificado de um servidor que você planeja adicionar como um nó de cluster de failover e, em seguida, selecione Adicionar. Repita essa etapa para cada servidor que quiser adicionar. Para adicionar vários servidores ao mesmo tempo, separe os nomes usando vírgula ou ponto e vírgula. Por exemplo, digite os nomes no formato server1.contoso.com; server2.contoso.com. Quando tiver terminado, selecione Avançar.

![criar cluster e selecionar servidores](media/ad-fs-always-on/createClusterServers.png)

> [!NOTE]
> Se você optar por criar o cluster imediatamente após executar a validação no [procedimento de validação de configuração](../../../failover-clustering/create-failover-cluster.md#validate-the-configuration), você não verá a página selecionar servidores. Os nós que foram validados são adicionados automaticamente ao Assistente de para Criação de Clusters para que você não precise inseri-los novamente.

6.  Se você tiver ignorado a validação anteriormente, a página Advertência de Validação será exibida. É expressamente recomendável realizar a validação do cluster. Apenas os clusters que são aprovados em todos os testes de validação são aceitos pela Microsoft. Para executar os testes de validação, selecione Sim e, em seguida, selecione avançar. Conclua o assistente para validar uma configuração conforme descrito em [validar a configuração](../../../failover-clustering/create-failover-cluster.md#validate-the-configuration).
7.  Na página Ponto de Acesso para Administrar o Cluster, faça o seguinte:
-   Na caixa Nome do Cluster, digite o nome que deseja usar para administrar o cluster. Antes disso, examine as seguintes informações:
 -  Durante a criação do cluster, esse nome é registrado como o objeto de computador do cluster (também conhecido como CNO ou objeto de nome do cluster) no AD DS. Se você especificar um nome NetBIOS para o cluster, o CNO será criado no mesmo local em que residem os objetos de computador dos nós do cluster. Isso pode ser o contêiner de computadores padrão ou uma OU.
 -  Para especificar um local diferente para o CNO, você pode digitar o nome diferenciado de uma OU na caixa Nome do Cluster. Por exemplo: CN=ClusterName, OU=Clusters, DC=Contoso, DC=com.
 -  Se um administrador do domínio tiver pré-configurado o CNO em uma OU diferente da qual residem os nós do cluster, especifique o nome diferenciado fornecido pelo administrador do domínio.
- Se o servidor não tiver um adaptador de rede configurado para usar protocolo DHCP, você deverá configurar um ou mais endereços IP estáticos para o cluster de failover. Marque a caixa de seleção ao lado de cada rede que deseja usar para o gerenciamento de cluster. Selecione o campo endereço ao lado de uma rede selecionada e, em seguida, insira o endereço IP que você deseja atribuir ao cluster. Esse endereço (ou endereços) IP será associado ao nome do cluster no DNS (Sistema de Nomes de Domínio).
- Quando tiver terminado, selecione Avançar.

8.  Na página Confirmação, examine as configurações. Por padrão, a caixa de seleção Adicione todo o armazenamento qualificado ao cluster é marcada. Desmarque essa caixa de seleção se:
-   Quiser configurar o armazenamento depois.
-   Estiver planejando criar espaços de armazenamento clusterizados por meio do Gerenciador de Cluster de Failover ou pelos cmdlets de Clustering de Failover do Windows PowerShell, e ainda não tiver criado espaços de armazenamento nos Serviços de Arquivo e Armazenamento. Para obter mais informações, consulte [Deploy Clustered Storage Spaces (Implantar espaços de armazenamento clusterizados)](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)).
9.  Selecione Avançar para criar o cluster de failover.
10. Na página Resumo, confirme se o cluster de failover foi criado com sucesso. Se houver avisos ou erros, exiba a saída de resumo ou selecione Exibir relatório para exibir o relatório completo. Selecione Concluir.
11. Para confirmar se o cluster foi criado, verifique se o nome do cluster está listado no Gerenciador de Cluster de Failover, na árvore de navegação. Você pode expandir o nome do cluster e, em seguida, selecionar itens em nós, armazenamento ou redes para exibir os recursos associados.
Observe que poderá levar algum tempo para que o nome do cluster seja replicado com sucesso no DNS. Após o registro de DNS e a replicação bem-sucedidos, se você selecionar todos os servidores no Gerenciador do Servidor, o nome do cluster deverá ser listado como um servidor com um status de gerenciamento online.

![criação de cluster concluída](media/ad-fs-always-on/createClusterComplete.png)

## <a name="enable-always-on-availability-groups-with-sql-server-configuration-manager"></a>Habilitar grupos de disponibilidade AlwaysOn com SQL Server Configuration Manager

1.  Conecte-se ao nó WSFC (cluster de failover do Windows Server) que hospeda a instância de SQL Server em que você deseja habilitar Always On grupos de disponibilidade.
2.  No menu Iniciar, aponte para todos os programas, aponte para Microsoft SQL Server, aponte para Ferramentas de Configuração e clique em SQL Server Configuration Manager.
3.  Em SQL Server Configuration Manager, clique em serviços SQL Server, clique com o botão direito do mouse em SQL Server ( <instance name> ), em que <instance name> é o nome de uma instância de servidor local para a qual você deseja habilitar Always on grupos de disponibilidade e clique em Propriedades.
4.  Selecione a guia Alta Disponibilidade AlwaysOn.
5.  Verifique se o campo Nome do cluster de failover do Windows contém o nome do cluster de failover local. Se esse campo estiver em branco, essa instância de servidor atualmente não oferece suporte a grupos de disponibilidade Always On. Ou o computador local não é um nó de cluster, o Cluster WSFC foi desligado ou esta edição do SQL Server que não oferece suporte a grupos de disponibilidade Always On.
6.  Marque a caixa de seleção Habilitar Grupos de Disponibilidade AlwaysOn e clique em OK.
O SQL Server Configuration Manager salva suas alterações. Em seguida, reinicie manualmente o serviço do SQL Server. Isso permite escolher a hora de reinicialização mais adequada de acordo com as necessidades da sua empresa. Quando o serviço de SQL Server for reiniciado, Always On será habilitado e a propriedade de servidor IsHadrEnabled será definida como 1.

![Habilitar AoA](media/ad-fs-always-on/enableAoAGroup.png)

## <a name="back-up-ad-fs-databases"></a>Fazer backup de bancos de dados AD FS
Faça backup da configuração do AD FS e dos bancos de dados de artefato com os logs de transação completos. Coloque o backup no destino escolhido.
Faça backup do artefato do ADFS e dos bancos de dados de configuração.
- Tarefas > backup > completo > adicionar a um arquivo de backup > OK para criar

![fazer backup do servidor](media/ad-fs-always-on/backUpADFS.png)

## <a name="create-new-availability-group"></a>Criar novo grupo de disponibilidade

1.  No Pesquisador de Objetos, conecte-se à instância do servidor que hospeda a réplica de disponibilidade primária.
2.  Expanda os nós Alta Disponibilidade AlwaysOn e Grupos de Disponibilidade.
3.  Para iniciar o Assistente de Novo Grupo de Disponibilidade, selecione o comando Assistente de Novo Grupo de Disponibilidade.
4.  Na primeira vez você executa este assistente, uma página de Introdução é exibida. Para ignorar essa página no futuro, clique em Não mostrar esta página novamente. Depois de ler esta página, clique em Avançar.
5.  Na página Especificar Opções do Grupo de Disponibilidade, insira o nome do novo grupo de disponibilidade no campo Nome do grupo de disponibilidade. Esse nome deve ser um identificador de SQL Server válido que seja exclusivo no cluster e no seu domínio como um todo. O tamanho máximo de um nome de grupo de disponibilidade é 128 caracteres. e
6.  Em seguida, especifique o tipo de cluster. Os tipos de cluster possíveis dependem da versão SQL Server e do sistema operacional. Escolha WSFC, EXTERNAL ou NONE. Para obter detalhes, consulte a página [especificar nome do grupo de disponibilidade](/sql/database-engine/availability-groups/windows/specify-availability-group-name-page?view=sql-server-ver15)

![nome AoA grupo e cluster](media/ad-fs-always-on/createAoAName.png)

7.  Na página Selecionar Bancos de Dados, a grade lista bancos de dados de usuário na instância do servidor conectada que estão qualificados para se tornarem os bancos de dados de disponibilidade. Selecione um ou mais dos bancos de dados listados para participarem do novo grupo de disponibilidade. Inicialmente, esses bancos de dados serão os bancos de dados primários iniciais.
Para cada banco de dados listado, a coluna Tamanho exibe o tamanho do banco de dados, se conhecido. A coluna status indica se um determinado banco de dados atende aos [pré-requisitos](/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability?view=sql-server-ver15) para bancos de dados de disponibilidade. Se os pré-requisitos não forem atendidos, uma descrição breve do status indicará o motivo pelo qual o banco de dados não se qualifica, por exemplo, se ele não usar o modelo de recuperação completa. Para obter mais informações, clique na descrição do status.
Se você alterar um banco de dados para torná-lo qualificado, clique em Atualizar para atualizar a grade de bancos de dados.
Se o banco de dados contiver uma chave mestra de banco de dados, digite a senha para a chave mestra de banco de dados na coluna Senha.

![selecionar bancos de dados para AoA](media/ad-fs-always-on/createAoASelectDb.png)

8. na página especificar réplicas, especifique e configure uma ou mais réplicas para o novo grupo de disponibilidade. Essa página contém quatro guias: A tabela a seguir apresenta essas guias. Para obter mais informações, consulte o tópico [especificar réplicas (Assistente de novo grupo de disponibilidade: Assistente para adicionar réplica)](/sql/database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard?view=sql-server-ver15) .

| Tab      | Breve descrição       |
| ------------------ |:-------------:|
| Réplicas     | Use essa guia para especificar cada instância do SQL Server que hospedará uma réplica secundária. Observe que a instância de servidor à qual você está conectado no momento deve hospedar a réplica primária. |
| Pontos de extremidade     | Use esta guia para verificar quaisquer pontos de extremidade de espelhamento de banco de dados existentes e também, se esse ponto de extremidade estiver ausente em uma instância de servidor cujas contas de serviço usam a Autenticação do Windows para criar o ponto de extremidade automaticamente.|
| Preferências de backup | Use esta guia para especificar sua preferência de backup para o grupo de disponibilidade como um todo e suas prioridades de backup para as réplicas de disponibilidade individuais.      |
| Ouvinte     | Use esta guia para criar um ouvinte de grupo de disponibilidade. Por padrão, o assistente não cria um ouvinte.      |

![especificar detalhes da réplica](media/ad-fs-always-on/createAoAchooseReplica.png)

9. Na página Selecionar Sincronização de Dados Inicial, escolha como você deseja que seus novos bancos de dados secundários sejam criados e unidos ao grupo de disponibilidade. Selecione uma das seguintes opções:
-   Propagação automática
 - O SQL Server cria automaticamente as réplicas secundárias para cada banco de dados no grupo. A propagação automática exige que os caminhos do arquivo de dados e de log sejam os mesmos em cada instância do SQL Server que faz parte do grupo. Disponível em SQL Server 2016 (13. x) e posterior. Consulte [inicializar automaticamente Always on grupos de disponibilidade](/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group?view=sql-server-ver15).
- Backup completo de log e de banco de dados
 - Selecione esta opção se o seu ambiente atender aos requisitos para iniciar automaticamente a sincronização de dados inicial (para obter mais informações, consulte [pré-requisitos, restrições e recomendações, anteriormente neste tópico)](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-ver15#Prerequisites).
Se você selecionar Total, depois de criar o grupo de disponibilidade, o assistente fará backup de todos os bancos de dados primários e de seu log de transações em um compartilhamento de rede e restaurará os backups em todas as instâncias de servidor que hospedam uma réplica secundária. Em seguida, o assistente unirá cada banco de dados secundário ao grupo de disponibilidade.
No campo Especifique um local de rede compartilhado acessível por todas as réplicas:, especifique um compartilhamento de backup para o qual todas as instâncias do servidor que hospedam réplicas de host têm acesso de leitura/gravação. Para obter mais informações, consulte Pré-requisitos anteriormente neste tópico. Na etapa de validação, o assistente executará um teste para garantir que o local de rede fornecido é válido, o teste criará um banco de dados na réplica primária denominada "BackupLocDb_" seguindo por um Guid e realizará backup no local de rede fornecido e, em seguida, o restaurará nas réplicas secundárias. É seguro excluir este banco de dados juntamente com seu histórico de backup e o arquivo de backup caso o assistente não pôde excluí-los.
- Somente junção
 - Se preparou bancos de dados secundários manualmente nas instâncias do servidor que hospedam réplicas secundárias, você poderá selecionar essa opção. O assistente unirá os bancos de dados secundários existentes ao grupo de disponibilidade.
- Ignorar sincronização de dados inicial
 - Selecione esta opção se desejar usar seus próprios backups de banco de dados e de log de seus bancos de dados primários. Para obter mais informações, consulte [Iniciar movimentação de dados em um banco de Always on secundário (SQL Server)](/sql/database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server?view=sql-server-ver15).

![escolher opção de sincronização de dados](media/ad-fs-always-on/createAoADataSync.png)

9.  A página Validação verifica se os valores especificados neste Assistente atendem aos requisitos do Assistente de Novo Grupo de Disponibilidade. Para fazer uma alteração, clique em Anterior para retornar a uma página anterior do assistente para alterar um ou mais valores. Clique em Avançar para retornar à página Validação e clique em Executar Novamente a Validação.

10. Na página Resumo, revise as opções escolhidas para o novo grupo de disponibilidade. Para fazer uma alteração, clique em Anterior para retornar à página relevante. Após fazer a alteração, clique em Avançar para retornar à página Resumo.

> [!NOTE]
> Quando a conta de serviço de SQL Server de uma instância de servidor que hospedará uma nova réplica de disponibilidade ainda não existir como um logon, o assistente de novo grupo de disponibilidade precisará criar o logon. Na página Resumo, o assistente exibe as informações para o logon que deve ser criado. Se você clicar em Concluir, o assistente criará esse logon para a conta de serviço do SQL Server e concederá a permissão CONNECT a ele.
> Se estiver satisfeito com a seleções, opcionalmente, clique em Script para criar um script das etapas que o assistente executará. Em seguida, para criar e configurar o novo grupo de disponibilidade, clique em Concluir.

11. A página Progresso exibe o andamento das etapas de criação do grupo de disponibilidade (configuração de pontos de extremidade, criação do grupo de disponibilidade e junção da réplica secundária ao grupo).
12. Quando essas etapas forem concluídas, a página Resultados exibirá o resultado de cada etapa. Se todas essas etapas tiverem êxito, o novo grupo de disponibilidade será configurado completamente. Se quaisquer das etapas resultar em um erro, você poderá precisar concluir a configuração manualmente ou use um assistente para a etapa com falha. Para obter informações sobre a causa de um determinado erro, clique no link de "Erro" associado na coluna Resultado.
Quando o assistente for concluído, clique em Fechar para sair.

![validação concluída](media/ad-fs-always-on/createAoAValidation.png)

## <a name="add-databases-on-secondary-node"></a>Adicionar bancos de dados no nó secundário

1.  Restaure o banco de dados de artefato por meio da interface do usuário no nó secundário usando os arquivos de backup criados.
![restaurar por meio da interface do usuário](media/ad-fs-always-on/restoreDB.png)

2. Restaure o banco de dados em um estado de não recuperação.
![restaurar com não recuperação](media/ad-fs-always-on/restoreNonRecovery.png)

3. Repita o processo para restaurar o banco de dados de configuração.

## <a name="join-availability-replica-to-an-availability-group"></a>Unir a réplica de disponibilidade a um grupo de disponibilidade

1.  No Pesquisador de Objetos, conecte-se à instância do servidor que hospeda a réplica secundária e clique no nome do servidor para expandir a árvore de servidores.
2.  Expanda os nós Alta Disponibilidade AlwaysOn e Grupos de Disponibilidade.
3.  Selecione o grupo de disponibilidade da réplica secundária à qual você está conectado.
4.  Clique com o botão direito do mouse na réplica secundária e clique em Unir a Grupo de Disponibilidade.
5.  Isso abre a caixa de diálogo Unir Réplica a Grupo de Disponibilidade.
6.  Para unir a réplica secundária ao grupo de disponibilidade, clique em OK.

![ingressar na réplica secundária](media/ad-fs-always-on/jointoAoA.png)

## <a name="update-the-sql-connection-string"></a>Atualizar a cadeia de conexão SQL
Por fim, use o PowerShell para editar as propriedades de AD FS para atualizar a cadeia de conexão SQL para usar o endereço DNS do ouvinte do grupo de disponibilidade AlwaysOn.
Execute a alteração do banco de dados de configuração em cada nó e reinicie o serviço ADFS em todos os nós do ADFS. O valor inicial do catálogo é alterado com base na versão do farm.

```
PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService
PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”
PS:\>$temp.put()
PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”
```
