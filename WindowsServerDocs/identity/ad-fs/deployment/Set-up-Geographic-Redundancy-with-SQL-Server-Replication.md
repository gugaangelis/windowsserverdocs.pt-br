---
title: Configurar a redundância geográfica com replicação do SQL Server
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: 00880d06835fad08538f634fdf2868146fc23b1a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442923"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Configurar a redundância geográfica com replicação do SQL Server


> [!IMPORTANT]  
> Se você quiser criar um farm do AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 ou superior.
  
Se você estiver usando o SQL Server como seu banco de dados de configuração do AD FS, você pode configurar geográfica\-redundância para seu farm do AD FS usando a replicação do SQL Server. Geográfica\-redundância replica dados entre dois sites geograficamente distantes, para que aplicativos podem alternar de um site para outro. Dessa forma, no caso de falha de um site, você pode ainda ter todos os dados de configuração disponíveis no segundo site. Para obter mais informações, consulte a seção de redundância geográfica"SQL Server" em [Federation Server Farm usando o SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Pré-requisitos  
Instalar e configurar um farm de servidores SQL. Para obter mais informações, consulte [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx). No SQL Server inicial, certifique-se de que o serviço SQL Server Agent está em execução e definido para inicialização automática.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Crie o segundo \(réplica\) SQL Server para geográfica\-redundância  
  
1. Instalar o SQL Server \(para obter mais informações, consulte [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copie os arquivos de script CREATEDB. SQL e SetPermissions.sql resultantes para o SQL server de réplica.  
  
2. Verifique se o serviço do SQL Server Agent está em execução e definido para inicialização automática  
  
3. Execute **exportar\-AdfsDeploymentSQLScript** no nó primário do AD FS para criar arquivos CREATEDB. SQL e SetPermissions.sql.  Por exemplo:`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql2.png)
  
4. Copie os scripts de servidor secundário.  Abra o script CREATEDB. SQL **SQL Management Studio** e clique em **Execute**.
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql4.png)

5. Abra o script SetPermissions.sql **SQL Management Studio** e clique em **Execute**.
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql6.png) 

   

> [!NOTE]
> Você também pode usar o seguinte na linha de comando. 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Criar configurações do publicador no SQL Server inicial  
  
1. Do SQL Server Management studio, sob **replicação**, clique com botão direito **publicações locais** e escolha **nova publicação... ** 
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql7.png) </br>  

2. Na tela do Assistente para nova publicação, clique em **próxima**.</br>
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql8.png) </br> 
  
3. Na **distribuidor** página, escolha o servidor local como o distribuidor e clique em **próxima**.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql9.png) </br>   

4. Sobre o **instantâneo** pasta insira \\\SQL1\repldata no lugar da pasta padrão. \(OBSERVAÇÃO: Talvez você precise criar esse compartilhamento por conta própria\).  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql10.png) </br>   
  
5. Escolher **AdfsConfigurationV3** como o banco de dados de publicação e clique em **próxima**.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql11.png) </br>
  
6. Na **tipo de publicação**, escolha **a publicação de mesclagem** e clique em **próxima**.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql12.png) </br>
  
7. Na **tipos de assinante**, escolha **SQL Server 2008 ou posterior** e clique em **próxima**.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql13.png) </br> 

8. Sobre o **artigos** página, selecione **tabelas** nó para selecionar todas as tabelas, em seguida, **un\-verificar SyncProperties** tabela \(este não deve ser replicado\)</br>
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql14.png) </br>    
  
9. Sobre o **artigos** página, selecione **funções definidas pelo usuário** nó para selecionar todas as funções de definidas pelo usuário e clique em **próxima**...  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql15.png) </br>    

10. Sobre o **problemas do artigo** página, clique em **próxima**.  
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql16.png) </br>   

11. Sobre o **filtrar linhas da tabela** , clique em **próxima**.  
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql17.png) </br>   
12. Sobre o **Snapshot Agent** página, escolha os padrões de imediato e 14 dias, clique em **próxima**.  
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql18.png) </br>   
    Você talvez precise criar uma conta de domínio para o SQL agent. Use as etapas em [logon do SQL de configurar para a conta de domínio CONTOSO\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) criar logon SQL para o novo usuário do AD e atribuir permissões específicas.  
  
13. No **segurança do agente** , clique em **configurações de segurança** e insira o nome de usuário\/senha de uma conta de domínio \(não é uma GMSA\) criado para o SQL agent e Clique em **Okey**.  Clique em **Avançar**.  
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql19.png) </br>  

14. Sobre o **ações do assistente** , clique em **próxima**.   
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql20.png) </br>

15. Sobre o **concluir o assistente** página, insira um nome para a publicação e clique em **concluir**. 
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql21.png) </br>  

16. Depois que a publicação é criada, você deve ver o status de sucesso.  Clique em **Fechar**.
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql22.png) </br>  

17. Novamente no SQL Server Management Studio, clique com botão direito na nova publicação e clique em **iniciar o Replication Monitor**.  
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Criar configurações de assinatura na réplica do SQL Server  
Certifique-se de que você criou as configurações do publicador no SQL Server inicial, conforme descrito acima e, em seguida, conclua o procedimento a seguir:  
  
1. Na réplica do SQL Server, do SQL Server Management studio, sob **replicação**, clique com botão direito **assinaturas locais** e escolha **nova assinatura...** . ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql24.png) </br>  

2. Sobre o **Assistente para nova assinatura** , clique em **próxima**.
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql25.png) </br>   
  
3. Sobre o **publicação** de página, selecione o Editor na lista suspensa.  Expandir **AdfsConfigurationV3** e selecione o nome da publicação criada acima e clique em **próxima**.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql26.png) </br> 
  
4. Sobre o **local do Merge Agent** página, selecione **executar cada agente em seu assinante \(assinaturas pull\)**  \(padrão\) e clique em  **Próxima**.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql27.png) </br> Isso, juntamente com o tipo de assinatura abaixo, determina a lógica de resolução de conflito. \(Para obter mais informações, consulte [detectar e resolver conflitos de replicação de mesclagem](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5. Sobre o **assinantes** página, selecione **AdfsConfigurationV3** como o banco de dados do assinante e clique em **próxima**.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql28.png) </br> 
  
6. Sobre o **segurança do Merge Agent** , clique em **...**  e insira o nome de usuário e a senha de uma conta de domínio \(não é uma GMSA\) criados para o SQL agent usando a caixa de elipses e clique em **próximo**.
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql30.png) </br> 
  
7. Na **agendamento de sincronização**, escolha **executar continuamente** e clique em **próxima**. 
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql31.png) </br> 
 
8. Na **inicializar assinaturas**, clique em **próxima**.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql32.png) </br> 
  
9. Na **tipo de assinatura**, escolha **cliente** e clique em **próxima**.  
  
   Implicações disso estão documentadas [aqui](https://technet.microsoft.com/library/ms151191.aspx) e [aqui](https://technet.microsoft.com/library/ms151170.aspx).  Essencialmente, podemos levar a resolução de conflitos "primeiro para o publicador vence" simples e não precisamos republicar para outros assinantes.  
   ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql33.png) </br>
   
10. No **ações do assistente** página, verifique se **criar a assinatura** está marcada e clique em **próxima**. 
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql34.png) </br>

11. Sobre o **concluir o assistente** , clique em **concluir**. 
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql35.png) </br>

12. Quando a assinatura tiver terminado o processo de criação, você deve ver o sucesso. Clique em **Fechar**. 
    ![Configurar a redundância geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Verifique se o processo de inicialização e de replicação  
  
1.  No SQL server primário, com o botão direito\-clique em de **replicação** nó e clique em **iniciar o Replication Monitor**.  
  
2.  Na **Replication Monitor**, clique na publicação.  
  
3.  Sobre o **todas as assinaturas** , clique com o botão direito e **exibir detalhes**.  
  
    Você deve ser capaz de ver o número de entradas sob **ações** para a replicação inicial.  
  
4.  Além disso, você pode examinar os **SQL Server Agent\\trabalhos** nó para ver o trabalho\(s\) agendado para executar as operações de publicação\/assinatura.  Somente os trabalhos locais são mostrados, certifique-se de verificar o publicador e assinante para solução de problemas.  À direita\-clique em um trabalho e selecione **exibir o histórico de** para exibir o histórico de execução e os resultados.  
  
## <a name="sqlagent"></a>Configurar o logon do SQL para a conta de domínio CONTOSO\\sqlagent  
  
1.  Criar um novo logon no primário e de réplica do SQL Server, chamada CONTOSO\\sqlagent \(o nome do novo usuário de domínio criados e configurados na **segurança do agente** página nos procedimentos acima.\)  
  
2.  No SQL Server, com o botão direito\-clique o logon que você criou e selecione Propriedades, em seguida, sob o **mapeamento de usuário** guia, mapear esse logon à **AdfsConfiguration** e **AdfsArtifact**  bancos de dados com o público e o banco de dados\_genevaservice funções. Também mapear esse logon para o banco de dados de distribuição e adicionar um BD\_função de proprietário para tabelas de distribuição e adfsconfiguration.  Faça isso conforme aplicável no SQL server primário e de réplica. Para obter mais informações, consulte [Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Dê a leitura de conta de domínio correspondente e permissões de gravação no compartilhamento configurado como distribuidor.  Certifique-se de que você defina leitura gravação e permissões nas permissões de compartilhamento e as permissões de arquivo local.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurar o AD FS nó\(s\) para apontar para o farm de réplica do SQL Server  
Agora que você configurou redundância geográfica, os nós do farm do AD FS podem ser configurados para apontar para seu farm do SQL Server de réplica usando os AD FS "associação" farm recursos standard, da interface do usuário do AD FS Configuration Wizard ou usando o Windows PowerShell.  
  
Se você usar a interface do usuário do AD FS configuração do assistente, selecione **adicionar um servidor de Federação a um farm de servidores de Federação**. **NÃO o fazem** selecionar **criar o primeiro servidor de Federação em um farm de servidores de Federação**.  
  
Se você usar o Windows PowerShell, execute **Add\-AdfsFarmNode**. **NÃO o fazem** executados **instale\-AdfsFarm**.  
  
Quando solicitado, forneça o nome de host e a instância do SQL Server, réplica **não** o inicial do SQL server.  
