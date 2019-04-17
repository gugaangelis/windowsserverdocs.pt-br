---
title: "Instalação redundância geográfica com replicação do SQL Server"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: a9f29c1eb19a8241eac5afb5c87581e6c988c3c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Instalação redundância geográfica com replicação do SQL Server

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

> [!IMPORTANT]  
> Se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 ou posterior.
  
Se você estiver usando o SQL Server como seu banco de dados de configuração do AD FS, você pode configurar geo\ redundância para seu farm AD FS usando replicação do SQL Server. Geo\ redundância replica os dados entre dois locais geograficamente distantes, para que os aplicativos podem alternar de um local para outro. Dessa forma, em caso de falha de um site, você ainda pode ter todos os dados de configuração disponíveis em segundo local. Para obter mais informações, consulte a seção de redundância geográfica"SQL Server" em [federação Server Farm usando o SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Pré-requisitos  
Instale e configure um farm de servidores SQL. Para obter mais informações, consulte [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). No SQL Server iniciais, certifique-se de que o serviço SQL Server Agent está em execução e defina como início automático.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Criar a segunda \(replica\) SQL Server para redundância geo\  
  
1.  Instalar o SQL Server \ (para obter mais informações, consulte [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copie os arquivos de script CreateDB.sql e SetPermissions.sql resultantes para o SQL server réplica.  
  
2.  Certifique-se de que está executando o serviço SQL Server Agent e defina como início automático  
  
3.  Executar **Export\ AdfsDeploymentSQLScript** no nó do AD FS principal para criar arquivos CreateDB.sql e SetPermissions.sql.  Por exemplo:`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql2.png)
  
4.  Copie os scripts de servidor secundário.  Abra o script CreateDB.sql na **SQL Management Studio** e clique em **Execute**.
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql4.png)

5. Abra o script SetPermissions.sql em **SQL Management Studio** e clique em **Execute**.
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql6.png) 

   

>[!NOTE]
>Você também pode usar o seguinte na linha de comando. 
>
>    `c:\>sqlcmd –i CreateDB.sql`  
>  
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Criar configurações de fornecedor no SQL Server inicial  
  
1.  Do SQL Server Management studio em **replicação**, clique com botão direito **publicações Local** e escolha **nova publicação... **
![ Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql7.png) </br>  

2.  Na tela novo Assistente de publicação clique **próxima**.</br>
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql8.png) </br> 
  
3.  Em **distribuidor** de página, escolha o servidor local como o distribuidor e clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql9.png) </br>   

4.  Sobre o **instantâneo** pasta página, insira \\\SQL1\repldata no lugar de pasta padrão. \ (Observação: talvez você precise criar esse compartilhamento yourself\).  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql10.png) </br>   
  
5.  Escolha **AdfsConfigurationV3** como o banco de dados de publicação e clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql11.png) </br>
  
6.  Em **tipo de publicação**, escolha **mesclar publicação** e clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql12.png) </br>
  
7.  Em **tipos de assinante**, escolha **SQL Server 2008 ou posterior** e clique em **próxima**.  
 ![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql13.png) </br> 

8.  No **artigos** página Selecione **tabelas** nó para selecionar todas as tabelas, em seguida, **un\ seleção SyncProperties** tabela \ (este não deve ser replicated\)</br>
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql14.png) </br>    
  
9.  Sobre o **artigos** página, selecione **funções definidas pelo usuário** nó para selecionar todas as funções de definidas pelo usuário e clique em **próxima**...  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql15.png) </br>    

10. Sobre o **problemas do artigo** página, clique **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql16.png) </br>   

11. Sobre o **linhas de tabela de filtro** página, clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql17.png) </br>   
12. Sobre o **agente instantâneo** página, escolher os padrões de imediatos e 14 dias, clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql18.png) </br>   
Talvez seja necessário criar uma conta de domínio para o agente do SQL. Use as etapas em [logon configurar SQL para a conta de domínio CONTOSO\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) criar logon do SQL para esse novo usuário AD e atribuir permissões específicas.  
  
13. No **agente segurança** página, clique em **configurações de segurança** e insira a username\/senha de uma conta de domínio \ (não um GMSA\) criado para o agente SQL e clique em **Okey**.  Clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql19.png) </br>  

14. Sobre o **ações do assistente** página, clique em **próxima**.   
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql20.png) </br>

15. Sobre o **concluir o assistente** página, insira um nome para sua publicação e clique em **concluir**. 
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql21.png) </br>  

16. Depois que a publicação é criada, você deve ver o status de sucesso.  Clique em **fechar**.
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql22.png) </br>  

17. No SQL Server Management Studio, clique com botão direito na nova publicação e clique em volta **iniciar o Monitor de replicação**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Criar configurações de inscrição da réplica do SQL Server  
Certifique-se de que você criou as configurações do fornecedor no SQL Server inicial conforme descrito acima e conclua o procedimento a seguir:  
  
1.  Na réplica SQL Server, do SQL Server Management studio, sob **replicação**, clique com botão direito **assinaturas Local** e escolha **nova assinatura... **. ![Configurar redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql24.png) </br>  

2.  Sobre o **novo Assistente de assinatura** página, clique em **próxima**.
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql25.png) </br>   
  
3.  Sobre o **publicação** página, selecione o fornecedor na lista suspensa.  Expanda **AdfsConfigurationV3** e selecione o nome da publicação criado anteriormente e clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql26.png) </br> 
  
4.  No **local de agente de mesclagem** página, selecione **executar cada agente ao seu assinante \(pull subscriptions\)** \(the default\) e clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql27.png) </br> Isso, juntamente com o tipo de assinatura abaixo, determina a lógica de resolução do conflito. \ (Para obter mais informações, consulte [detectar e resolver conflitos de replicação mesclar](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5.  Sobre o **assinantes** página, selecione **AdfsConfigurationV3** como o banco de dados do assinante e clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql28.png) </br> 
  
6.  Sobre o **segurança de agente de mesclagem** página, clique em **... ** e insira o nome de usuário e a senha de uma conta de domínio \ (não um GMSA\) é criado para o agente do SQL usando a caixa elipses e e clique em **próxima**.
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql30.png) </br> 
  
7.  Em **cronograma de sincronização**, escolha **executados continuamente** e clique em **próxima**. 
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql31.png) </br> 
 
8.  Em **inicializar assinaturas**, clique em **próxima**.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql32.png) </br> 
  
9.  Em **tipo de assinatura**, escolha **cliente** e clique em **próxima**.  
  
    Implicações de isso estão documentadas [aqui](https://technet.microsoft.com/library/ms151191.aspx) e [aqui](https://technet.microsoft.com/library/ms151170.aspx).  Basicamente, pegamos a resolução de conflitos de "primeiro editor vence" simples e nós não precisará republicar para outros assinantes.  
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql33.png) </br>
   
10. Sobre o **ações do assistente** página, certifique-se de **criar a assinatura** está marcada e clique em **próxima**. 
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql34.png) </br>

11. Sobre o **concluir o assistente** página, clique em **concluir**. 
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql35.png) </br>

12. Depois que a assinatura termina o processo de criação, você deve ver sucesso. Clique em **fechar**. 
![Configurar a redundância geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Verifique se o processo de inicialização e replicação  
  
1.  No SQL server principal, clique right\ o **replicação** nó e clique em **iniciar o Monitor de replicação**.  
  
2.  Em **Monitor de replicação**, clique na publicação.  
  
3.  Sobre o **todas as assinaturas** guia, clique com botão direito e **exibir detalhes**.  
  
    Você deve ser capaz de ver várias entradas em **ações** para a replicação inicial.  
  
4.  Além disso, você pode examinar sob o **SQL Server Agent\\Jobs** nó para ver o job\(s\) agendada para executar as operações da publication\/assinatura.  Somente locais trabalhos são mostrados, portanto certifique-se de verificar o fornecedor e o assinante para solução de problemas.  Um trabalho Right\ pressionada e selecione **Exibir histórico** para exibir o histórico de execução e resultados.  
  
## <a name="sqlagent"></a>Configurar o logon do SQL para a conta de domínio CONTOSO\\sqlagent  
  
1.  Criar um novo logon em primária e réplica SQL Server chamada CONTOSO\\sqlagent \ (o nome do novo usuário domínio criados e configurados no **agente segurança** página nos procedimentos acima. \)  
  
2.  No SQL Server, clique right\ o logon, você criou e selecione Propriedades, em seguida, sob o **User Mapping** guia, mapear este logon para **AdfsConfiguration** e **AdfsArtifact** bancos de dados com funções de público e db\_genevaservice. Também mapear este logon ao banco de dados de distribuição e adicione a função db\_owner para distribuição e tabelas de adfsconfiguration.  Fazer isso conforme aplicável em principal e réplica SQL server. Para obter mais informações, consulte [modelo de segurança do agente de replicação](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Dê a leitura de conta de domínio correspondente e permissões de gravação no compartilhamento de configurado como distribuidor.  Certifique-se de que você defina ler e gravar permissões sob as permissões de compartilhamento e as permissões de arquivo local.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurar o AD FS node\(s\) para apontar para o farm de réplica do SQL Server  
Agora que você configurou redundância de localização geográfica, os nós do AD FS farm podem ser configurados para apontar para seu farm do SQL Server réplica usando os AD FS "ingresso" farm recursos padrão, a partir da interface do usuário do AD FS Configuration Wizard ou usando o Windows PowerShell.  
  
Se você usar a interface do usuário do AD FS Configuration Wizard, selecione **adicionar um servidor de federação para um farm de servidores de Federação**. **Não** selecione **criar o primeiro servidor de Federação em um farm de servidores de Federação**.  
  
Se você usar o Windows PowerShell, execute **Add\ AdfsFarmNode**. **Não** executar **Install\ AdfsFarm**.  
  
Quando solicitado, forneça o nome de host e a instância da réplica SQL Server, **não** o SQL server inicial.  
