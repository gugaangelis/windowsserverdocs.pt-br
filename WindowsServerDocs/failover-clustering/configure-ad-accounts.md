---
title: Configuração de contas de cluster no Active Directory
ms.date: 11/12/2012
ms.prod: windows-server
ms.technology: storage-failover-clustering
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 8a540361cdd07f6adfc1c929d77c510ef8433d6d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369887"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>Configuração de contas de cluster no Active Directory

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

No Windows Server, quando você cria um cluster de failover e configura serviços ou aplicativos em cluster, os assistentes de cluster de failover criam as contas de computador Active Directory necessárias (também chamadas de objetos de computador) e concedem a elas permissões específicas. Os assistentes criam uma conta de computador para o cluster em si (essa conta também é chamada de objeto de nome do cluster ou CNO) e uma conta de computador para a maioria dos tipos de aplicativos e serviços clusterizados, com exceção de uma máquina virtual Hyper-V. As permissões para essas contas são definidas automaticamente pelos assistentes de cluster de failover. Se as permissões forem modificadas, será preciso alterá-las de volta para que correspondam aos requisitos do cluster. Este guia descreve essas contas e permissões do Active Directory, fornece informações históricas sobre a importância delas e descreve as etapas para configurar e gerenciar as contas.
      

## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>Visão geral das contas do Active Directory necessárias para um cluster de failover

Esta seção descreve as contas de computador do Active Directory (também chamadas de objetos de computador do Active Directory) que são importantes para um cluster de failover. As contas são estas:

  - **A conta de usuário usada para criar o cluster.** Essa é a conta de usuário usada para iniciar o assistente para Criação de Clusters. A conta é importante porque fornece a base na qual uma conta de computador é criada para o cluster em si.  
      
  - **A conta de nome do cluster.** (a conta de computador do próprio cluster, também chamada de objeto de nome de cluster ou CNO). Essa conta é criada automaticamente pelo assistente para Criação de Clusters e apresenta o mesmo nome do cluster. A conta de nome do cluster é muito importante, pois por meio dessa conta, outras contas são automaticamente criadas conforme você configura novos serviços e aplicativos no cluster. Se a conta de nome do cluster for excluída ou se suas permissões forem removidas, outras contas não poderão ser criadas conforme exigidas pelo cluster até que a conta de nome do cluster seja restaurada ou as permissões corretas sejam restabelecidas.  
      
    Por exemplo, se você criar um cluster chamado Cluster1 e tentar configurar um servidor de impressão clusterizado chamado PrintServer1 em seu cluster, a conta de Cluster1 no Active Directory precisará reter as permissões corretas, de forma que ela possa ser usada para criar uma conta de computador chamada PrintServer1.  
      
    A conta de nome do cluster é criada no contêiner padrão de contas de computador no Active Directory. Por padrão, esse contêiner é "Computadores", mas o administrador de domínio pode optar por redirecioná-la para outro contêiner ou outra unidade organizacional.  
      
  - **A conta de computador (objeto de computador) de um serviço ou aplicativo em cluster.** Essas contas são criadas automaticamente pelo assistente para Alta Disponibilidade como parte do processo de criação da maioria dos tipos de aplicativo ou serviços clusterizados, com exceção de uma máquina virtual Hyper-V. A conta de nome de cluster recebe as permissões necessárias para controlar essas contas.  
      
    Por exemplo, se você tiver um cluster chamado Cluster1 e criar um servidor de arquivos clusterizado chamado FileServer1, o assistente para Alta Disponibilidade criará uma conta de computador do Active Directory chamada FileServer1. O assistente de alta disponibilidade também fornece à conta CLUSTER1 as permissões necessárias para controlar a conta FileServer1.  
      

A tabela a seguir descreve as permissões necessárias para essas contas.


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Conta</th>
<th>Detalhes sobre permissões</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Conta usada para criar o cluster</p></td>
<td><p>Exige permissões administrativas nos servidores que se tornarão nós do cluster. Também exige as permissões <strong>Criar Objetos de computador</strong> e <strong>Ler Todas as Propriedades</strong> no contêiner que é usado para contas de computador no domínio.</p></td>
</tr>
<tr class="even">
<td><p>Conta de nome do cluster (conta de computador do próprio cluster)</p></td>
<td><p>Quando o assistente para Criação de Clusters é executado, ele cria a conta de nome do cluster no contêiner padrão que é usado para contas de computador no domínio. Por padrão, a conta de nome do cluster (assim como outras contas de computador) pode criar até dez contas de computador no domínio.</p>
<p>Se você criar a conta de nome do cluster (objeto de nome do cluster) antes de criar o cluster, isto é, pré-preparar a conta, será preciso fornecer a ela as permissões <strong>Criar Objetos de computador</strong> e <strong>Ler Todas as Propriedades</strong> no contêiner que é usado para contas de computador no domínio. Também será preciso desabilitar a conta e fornecer <strong>Controle Total</strong> dela à conta que será usada pelo administrador que instalará o cluster. Para obter mais informações, consulte <a href="#steps-for-prestaging-the-cluster-name-account" data-raw-source="[Steps for prestaging the cluster name account](#steps-for-prestaging-the-cluster-name-account)">Etapas para pré-preparar a conta de nome do cluster</a>, mais adiante neste guia.</p></td>
</tr>
<tr class="odd">
<td><p>Conta de computador de um aplicativo ou serviço clusterizado</p></td>
<td><p>Quando o assistente de alta disponibilidade é executado (para criar um novo serviço ou aplicativo em cluster), na maioria dos casos, uma conta de computador para o serviço ou aplicativo clusterizado é criada no Active Directory. A conta de nome de cluster recebe as permissões necessárias para controlar essa conta. A exceção é uma máquina virtual clusterizada do Hyper-V: nenhuma conta de computador foi criada para isso.</p>
<p>Se você pré-configurar a conta de computador para um serviço ou aplicativo em cluster, deverá configurá-la com as permissões necessárias. Para obter mais informações, consulte <a href="#steps-for-prestaging-an-account-for-a-clustered-service-or-application" data-raw-source="[Steps for prestaging an account for a clustered service or application](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)">Etapas para pré-preparar uma conta para um aplicativo ou serviço clusterizado</a>, mais adiante neste guia.</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> Em versões anteriores do Windows Server, havia uma conta para o Serviço de cluster. Como o Windows Server 2008, no entanto, o Serviço de cluster é executado automaticamente em um contexto especial que fornece permissões e privilégios específicos necessários para o serviço (semelhante ao contexto do sistema local, mas com privilégios reduzidos). No entanto, são necessárias outras contas, conforme descrito neste guia. 
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>Como as contas são criadas por meio de assistentes no clustering de failover

O diagrama a seguir ilustra o uso e a criação de contas de computador (objetos do Active Directory) que são descritos na subseção anterior. Essas contas entram em ação quando um administrador executa o assistente para Criação de Clusters e, em seguida, executa o assistente para Alta Disponibilidade (para configurar um aplicativo ou serviço clusterizado).

![](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

Observe que o diagrama acima mostra um único administrador executando ambos os assistentes - Criação de Clusters e Alta Disponibilidade. No entanto, isso poderia ser feito por dois administradores usando duas contas de usuário diferentes, se ambas as contas tivessem permissões suficientes. As permissões são descritas mais detalhadamente em requisitos relacionados a clusters de failover, Active Directory domínios e contas, mais adiante neste guia.

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>Como os problemas podem surgir se as contas das quais o cluster precisa forem alteradas

O diagrama a seguir ilustra como os problemas podem surgir se a conta de nome do cluster (uma das contas exigidas pelo cluster) for alterada depois que for automaticamente criada pelo assistente para Criação de Clusters.

![](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

Se ocorrer o tipo de problema mostrado no diagrama, um determinado evento (1193, 1194, 1206 ou 1207) será registrado em log no Visualizador de Eventos. Para obter mais informações sobre esses eventos, [http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271)consulte.

Observe que poderá ocorrer um problema semelhante com a criação de uma conta para um aplicativo ou serviço clusterizado se a cota de todo o domínio para criação de objetos de computador (por padrão, 10) for atingida. Nesse caso, pode ser adequado consultar o administrador de domínio para aumentar a cota, embora essa seja uma configuração de todo o domínio e deva ser alterada somente depois de uma análise detalhada, e somente depois de confirmar que o diagrama anterior não descreve sua situação. Para obter mais informações, consulte [Etapas para solucionar problemas causados por alterações em contas do Active Directory relacionadas a cluster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts), mais adiante neste guia.

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>Requisitos relacionados a clusters de failover, domínios do Active Directory e contas

Conforme descrito nas três seções anteriores, determinados requisitos devem ser atendidos para que aplicativos e serviços clusterizados possam ser configurados com êxito em um cluster de failover. Os requisitos mais básicos referem-se ao local dos nós do cluster (em um único domínio) e ao nível de permissões da conta da pessoa que instala o cluster. Se esses requisitos forem atendidos, as outras contas exigidas pelo cluster poderão ser criadas automaticamente pelos assistentes de cluster de failover. A lista a seguir fornece detalhes sobre esses requisitos básicos.

  - **Nós** todos os nós devem estar no mesmo domínio do Active Directory. (O domínio não pode se basear no Windows NT 4.0, que não inclui o Active Directory.)  
      
  - **Conta da pessoa que instala o cluster:** a pessoa que instala o cluster deve usar uma conta com as seguintes características:  
      
      - A conta deve ser uma conta de domínio. Ela não precisa ser uma conta de administrador de domínio. Ela poderá ser uma conta de usuário de domínio se atender a outros requisitos desta lista.  
          
      - A conta deve ter permissões administrativas nos servidores que se tornarão nós do cluster. O modo mais simples de fazer isso é criar uma conta de usuário de domínio e adicioná-la ao grupo local Administradores em cada um dos servidores que se tornará nó do cluster. Para obter mais informações, consulte [Etapas para configurar a conta para a pessoa que instala o cluster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), mais adiante neste guia.  
          
      - A conta (ou o grupo do qual a conta é um membro) deve receber as permissões **Criar Objetos de computador** e **Ler Todas as Propriedades** no contêiner que é usado para contas de computador no domínio. Para obter mais informações, consulte [Etapas para configurar a conta para a pessoa que instala o cluster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), mais adiante neste guia.  
          
      - Se sua organização optar por pré-preparar a conta de nome do cluster (uma conta de computador com o mesmo nome do cluster), essa conta pré-preparada deverá fornecer a permissão "Controle Total" à conta da pessoa que instala o cluster. Para obter outros detalhes importantes sobre como pré-preparar a conta de nome do cluster, consulte [Etapas para pré-preparar a conta de nome do cluster](#steps-for-prestaging-the-cluster-name-account), mais adiante neste guia.  
          

### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>Planejando redefinições de senha e outra manutenção de conta com antecipação

Às vezes, os administradores de clusters de failover podem precisar redefinir a senha da conta de nome do cluster. Essa ação exige uma permissão específica, **Redefinir senha**. Portanto, é uma prática recomendada editar as permissões da conta de nome do cluster (usando o snap-in Usuários e Computadores do Active Directory) para fornecer aos administradores do cluster a permissão **Redefinir senha** para a conta de nome do cluster. Para obter mais informações, consulte [Etapas para solucionar problemas de senha com a conta de nome do cluster](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account), mais adiante neste guia.

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>Etapas para configurar a conta para a pessoa que instala o cluster

A conta da pessoa que instala o cluster é importante porque fornece a base na qual uma conta de computador é criada para o cluster em si.

A associação de grupo mínima exigida para concluir o procedimento a seguir depende se você está criando a conta de domínio e cedendo a ela as permissões exigidas no domínio ou se você está apenas colocando a conta (criada por outra pessoa) no grupo local **Administradores** nos servidores que serão nós do cluster de failover. Se o primeiro, a associação em **operadores de conta** ou equivalente, é o mínimo necessário para concluir este procedimento. Se for a segunda o opção, é preciso apenas a associação ao grupo local **Administradores** nos servidores que serão nós no cluster de failover, ou equivalente. Examine os detalhes sobre como usar as contas e associações de [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)grupo apropriadas em.

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>Para configurar a conta para a pessoa que instala o cluster

1.  Crie ou obtenha uma conta de domínio para a pessoa que instala o cluster. Essa conta pode ser uma conta de usuário de domínio ou uma conta de **operadores de conta** . Se você usar uma conta de usuário padrão, terá que lhe dar algumas permissões adicionais posteriormente neste procedimento.

2.  Se a conta que foi criada ou obtida na etapa 1 não for incluída automaticamente no grupo de **Administradores** locais em computadores no domínio, adicione a conta ao grupo de **Administradores** locais nos servidores que serão nós no failover em
    
    1.  Clique em **Iniciar**, clique em **Ferramentas Administrativas** e clique em **Gerenciador do Servidores**.  
          
    2.  Na árvore de console, expanda **Configuração**, expanda **Usuários e Grupos Locais** e, por fim, expanda **Grupos**.  
          
    3.  No painel central, clique com o botão direito do mouse em **Administradores**, clique em **Adicionar ao Grupo** e em **Adicionar**.  
          
    4.  Em **Inserir os nomes de objeto a serem selecionados**, digite o nome da conta de usuário que foi criada ou obtida na etapa 1. Se solicitado, insira um nome de conta e senha com permissões suficientes para esta ação. Em seguida, clique em **OK**.  
          
    5.  Repita essas etapas em cada servidor que será um nó no cluster de failover.  

> [!IMPORTANT]
> Essas etapas devem ser repetidas em todos os servidores que serão nós no cluster. 
<br>


3. Se a conta que foi criada ou obtida na etapa 1 for uma conta de administrador de domínio, ignore o restante deste procedimento. Caso contrário, forneça à conta as permissões **Criar Objetos de computador** e **Ler Todas as Propriedades** no contêiner que é usado para contas de computador no domínio:
    
   1.  Em um controlador de domínio, clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**. Se a caixa de diálogo **Controle de Conta de Usuário** for exibida, confirme se a ação exibida é a desejada e clique em **Continuar**.  
          
   2.  No menu **Exibir**, verifique se a opção **Recursos Avançados** está selecionada.  
          
       Quando **Recursos Avançados** estiver selecionada, você poderá ver a guia **Segurança** nas propriedades de contas (objetos) em **Usuários e Computadores do Active Directory**.  
          
   3.  Clique com o botão direito do mouse no contêiner padrão **Computadores** ou no contêiner padrão no qual as contas de computador são criadas no seu domínio e clique em **Propriedades**. Os **computadores** estão localizados em <b>Active Directory usuários e computadores/</b><i>domínio-nó</i><b>/Computers</b>.  
          
   4.  Na guia **Segurança** , clique em **Avançado**.  
          
   5.  Clique em **Adicionar**, digite o nome da conta que foi criada ou obtida na etapa 1 e clique em **OK**.  
          
   6.  Na caixa de diálogo **entrada de permissão para o contêiner * ** *, localize as permissões **criar objetos de computador** e **ler todas as propriedades** e verifique se a caixa de seleção **permitir** está marcada para cada uma delas.  
          
       ![](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>Etapas para pré-preparar a conta de nome do cluster

Normalmente, o processo se torna mais simples se você não pré-preparar a conta de nome do cluster, mas em vez disso, permitir que a conta seja criada e configurada automaticamente quando você executa o assistente para Criação de Clusters. No entanto, se for necessário pré-preparar a conta de nome do cluster devido aos requisitos em sua organização, use o procedimento a seguir.

A associação ao grupo **Admins. do Domínio** , ou equivalente, é o mínimo necessário para concluir este procedimento. Examine os detalhes sobre como usar as contas e associações de [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)grupo apropriadas em. Observe que é possível usar a mesma conta para este procedimento que você usará ao criar o cluster.

#### <a name="to-prestage-a-cluster-name-account"></a>Para pré-preparar uma conta de nome do cluster

1.  Você precisa saber o nome que o cluster terá, bem como o nome da conta de usuário que será usada pela pessoa que criará o cluster. (Observe que é possível usar essa conta para executar este procedimento.)

2.  Em um controlador de domínio, clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**. Se a caixa de diálogo **Controle de Conta de Usuário** for exibida, confirme se a ação exibida é a desejada e clique em **Continuar**.

3.  Na árvore de console, clique com o botão direito do mouse em **Computadores** ou no contêiner padrão no qual as contas de computador são criadas no seu domínio. Os **computadores** estão localizados em <b>Active Directory usuários e computadores/</b><i>domínio-nó</i><b>/Computers</b>.

4.  Clique em **Novo** e em **Computador**.

5.  Digite o nome que será usado para o cluster de failover; em outras palavras, o nome do cluster que será especificado no assistente para Criação de Clusters e clique em **OK**.

6.  Clique com o botão direito do mouse na conta recém-criada e clique em **Desabilitar Conta**. Se for solicitado que você confirme sua escolha, clique em **Sim**.
    
    A conta deve ser desabilitada para que, quando o assistente para **Criação de Clusters** for executado, ele possa confirmar que a conta que será usada para o cluster não esteja sendo usada no momento por um computador ou cluster existente no domínio.

7.  No menu **Exibir**, verifique se a opção **Recursos Avançados** está selecionada.
    
    Quando **Recursos Avançados** estiver selecionada, você poderá ver a guia **Segurança** nas propriedades de contas (objetos) em **Usuários e Computadores do Active Directory**.

8.  Clique com o botão direito do mouse na pasta que você clicou com o botão direito do mouse na etapa 3 e clique em **Propriedades**.

9.  Na guia **Segurança** , clique em **Avançado**.

10. Clique em **Adicionar**, em **Tipos de Objeto**, verifique se a opção **Computadores** está selecionada e clique em **OK**. Em **Digite o nome do objeto a ser selecionado**, digite o nome da conta de computador que acabou de ser criada e clique em **OK**. Se uma mensagem for exibida informando que você está prestes a adicionar um objeto desabilitado, clique em **OK**.

11. Na caixa de diálogo **Entrada de Permissão**, localize as permissões **Criar Objetos de computador** e **Ler Todas as Propriedades** e verifique se a caixa de seleção **Permitir** está marcada para cada uma delas.
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. Clique em **OK** até retornar ao snap-in **Usuários e Computadores do Active Directory**.

13. Se para executar este procedimento, você estiver usando a mesma conta que será usada para criar o cluster, ignore as etapas restantes. Caso contrário, você deve configurar as permissões para que a conta de usuário que será usada para criar o cluster tenha controle total da conta de computador recém-criada:
    
    1.  No menu **Exibir**, verifique se a opção **Recursos Avançados** está selecionada.  
          
    2.  Clique com o botão direito do mouse na conta de computador recém-criada e clique em **Propriedades**.  
          
    3.  Na guia **Segurança**, clique em **Adicionar**. Se a caixa de diálogo **Controle de Conta de Usuário** for exibida, confirme se a ação exibida é a desejada e clique em **Continuar**.  
          
    4.  Use a caixa de diálogo **Selecionar Usuários, Computadores ou Grupos** para especificar a conta de usuário que será usada ao criar o cluster. Em seguida, clique em **OK**.  
          
    5.  Verifique se a conta de usuário que você acabou adicionar está selecionada e, próximo a **Controle Total**, marque a caixa de seleção **Permitir**.  
          
        ![](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>Etapas para pré-preparar uma conta para um aplicativo ou serviço clusterizado

Normalmente, o processo se torna mais simples se você não pré-preparar a conta de computador para um aplicativo ou serviço clusterizado, mas em vez disso, permitir que a conta seja criada e configurada automaticamente quando você executa o assistente para Alta Disponibilidade. No entanto, se for necessário pré-preparar as contas devido aos requisitos em sua organização, use o procedimento a seguir.

A associação ao grupo **Operadores de Conta**, ou equivalente, é o mínimo necessário para concluir este procedimento. Examine os detalhes sobre como usar as contas e associações de [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)grupo apropriadas em.

#### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>Para pré-preparar uma conta para um aplicativo ou serviço clusterizado

1.  Você precisa saber o nome do cluster e o nome que o aplicativo ou serviço clusterizado terá.

2.  Em um controlador de domínio, clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**. Se a caixa de diálogo **Controle de Conta de Usuário** for exibida, confirme se a ação exibida é a desejada e clique em **Continuar**.

3.  Na árvore de console, clique com o botão direito do mouse em **Computadores** ou no contêiner padrão no qual as contas de computador são criadas no seu domínio. Os **computadores** estão localizados em <b>Active Directory usuários e computadores/</b><i>domínio-nó</i><b>/Computers</b>.

4.  Clique em **Novo** e em **Computador**.

5.  Digite o nome que você usará para o aplicativo ou serviço clusterizado e clique em **OK**.

6.  No menu **Exibir**, verifique se a opção **Recursos Avançados** está selecionada.
    
    Quando **Recursos Avançados** estiver selecionada, você poderá ver a guia **Segurança** nas propriedades de contas (objetos) em **Usuários e Computadores do Active Directory**.

7.  Clique com o botão direito do mouse na conta de computador recém-criada e clique em **Propriedades**.

8.  Na guia **Segurança**, clique em **Adicionar**.

9.  Clique em **Tipos de Objeto**, verifique se a opção **Computadores** está selecionada e clique em **OK**. Em **Digite o nome do objeto a ser selecionado**, digite a conta de nome do cluster e clique em **OK**. Se uma mensagem for exibida informando que você está prestes a adicionar um objeto desabilitado, clique em **OK**.

10. Verifique se a conta de nome do cluster está selecionada e, próximo a **Controle Total**, marque a caixa de seleção **Permitir**.
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>Etapas para solucionar problemas relacionados às contas usadas pelo cluster

Conforme descrito anteriormente neste guia, quando você cria um cluster de failover e configura aplicativos ou serviços clusterizados, os assistentes de cluster de failover criam as contas do Active Directory necessárias e fornecem a elas as permissões corretas. Se uma conta necessária for excluída, ou as permissões necessárias forem alteradas, isso poderá ocasionar problemas. As subseções a seguir fornecem as etapas para solucionar esses problemas.

  - [Etapas para solucionar problemas de senha com a conta de nome de cluster](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)
      
  - [Etapas para solucionar problemas causados por alterações em contas de Active Directory relacionadas ao cluster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)
      

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>Etapas para solucionar problemas de senha com a conta de nome do cluster

Use este procedimento se houver uma mensagem de evento sobre objetos de computador ou sobre a identidade do cluster que inclua o texto a seguir. Observe que este texto estará na mensagem do evento, e não no início dela:

`Logon failure: unknown user name or bad password.`

As mensagens de evento que se enquadram na descrição acima indicam que a senha para a conta de nome do cluster e a senha correspondente armazenada pelo software de clustering não correspondem.

Para obter informações sobre como garantir que os administradores de cluster tenham as permissões corretas para executar o procedimento a seguir conforme necessário, consulte Planejamento antecipado para redefinições de senha e outra manutenção de conta, anteriormente neste guia.

A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento. Além disso, sua conta deve receber a permissão **Redefinir senha** para a conta de nome do cluster (a menos que conta seja uma conta de **Admins. de Domínio** ou seja o Proprietário Criador da conta de nome do cluster). A conta que foi usada pela pessoa que instalou o cluster pode ser usada para este procedimento. Examine os detalhes sobre como usar as contas e associações de [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)grupo apropriadas em.

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>Para solucionar problemas de senha com a conta de nome do cluster

1.  Para abrir o snap-in de cluster de failover, clique em **Iniciar**, **Ferramentas Administrativas** e em **Gerenciamento de Cluster de Failover**. (Se a caixa de diálogo **controle de conta de usuário** for exibida, confirme se a ação exibida é o que você deseja e clique em **continuar**.)

2.  No snap-in Gerenciamento de Cluster de Failover, se o cluster que você deseja configurar não for exibido, na árvore de console, clique com o botão direito em **Gerenciamento de Cluster de Failover**, clique em **Gerenciar um Cluster** e selecione ou especifique o cluster desejado.

3.  No painel central, expanda **Recursos Principais de Cluster**.

4.  Em **Nome do Cluster**, clique com o botão direito do mouse no item **Nome**, aponte para **Mais Ações** e clique em **Reparar Objeto do Active Directory**.

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Etapas para solucionar problemas causados por alterações nas contas do Active Directory relacionadas ao cluster

Se a conta de nome do cluster for excluída ou se suas permissões forem removidas, os problemas ocorrerão quando você tentar configurar um novo aplicativo ou serviço clusterizado. Para solucionar um problema cuja causa seja essa, use o snap-in Usuários e Computadores do Active Directory para exibir ou alterar a conta de nome do cluster e outras contas relacionadas. Para obter informações sobre os eventos que são registrados quando esse tipo de problema ocorre (evento 1193, 1194, 1206 ou 1207), consulte [http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271).

A associação ao grupo **Admins. do Domínio** , ou equivalente, é o mínimo necessário para concluir este procedimento. Examine os detalhes sobre como usar as contas e associações de [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)grupo apropriadas em.

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Para solucionar problemas causados por alterações nas contas do Active Directory relacionadas ao cluster

1.  Em um controlador de domínio, clique em **Iniciar**, **Ferramentas Administrativas** e em **Usuários e Computadores do Active Directory**. Se a caixa de diálogo **Controle de Conta de Usuário** for exibida, confirme se a ação exibida é a desejada e clique em **Continuar**.

2.  Expanda o contêiner padrão **Computadores** ou a pasta na qual a conta de nome do cluster (a conta de computador para o cluster) está localizada. Os **computadores** estão localizados em <b>Active Directory usuários e computadores/</b><i>domínio-nó</i><b>/Computers</b>.

3.  Examine o ícone da conta de nome do cluster. Ele não deve ter uma seta apontando para baixo, isto é, a conta não deve estar desabilitada. Se ela aparecer como desabilitada, clique nela com o botão direito do mouse e procure o comando **Habilitar Conta**. Ao visualizar o comando, clique nele.

4.  No menu **Exibir**, verifique se a opção **Recursos Avançados** está selecionada.
    
    Quando **Recursos Avançados** estiver selecionada, você poderá ver a guia **Segurança** nas propriedades de contas (objetos) em **Usuários e Computadores do Active Directory**.

5.  Clique com o botão direito do mouse no contêiner padrão **Computadores** ou na pasta em que a conta de nome do cluster está localizada.

6.  Clique em **Propriedades**.

7.  Na guia **Segurança** , clique em **Avançado**.

8.  Na lista de contas com permissões, clique na conta de nome do cluster e em **Editar**.
    

> [!NOTE]
> Se a conta de nome do cluster não estiver na lista, clique em <STRONG>Adicionar</STRONG> para adicioná-la. 
<br>


9. Para a conta de nome do cluster (também conhecida como o objeto de nome do cluster ou CNO), verifique se a opção **Permitir** está selecionada para as permissões **Criar Objetos de computador** e **Ler Todas as Propriedades**.
    
   ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. Clique em **OK** até retornar ao snap-in **Usuários e Computadores do Active Directory**.

11. Analise as políticas de domínio (consultando um administrador de domínio, se possível) relacionadas à criação de contas de computador (objetos). Verifique se a conta de nome do cluster pode criar uma conta de computador toda vez que você configura um aplicativo ou serviço clusterizado. Por exemplo, se seu administrador de domínio definiu configurações que fazem com que todas as novas contas de computador sejam criadas em um contêiner especializado, e não no contêiner padrão **Computadores**, verifique se essas configurações permitem que a conta de nome do cluster crie novas contas de computador também nesse contêiner.

12. Expanda o contêiner padrão **Computadores** ou o contêiner no qual a conta de computador para um dos aplicativos ou serviços clusterizados está localizada.

13. Clique com o botão direito do mouse na conta de computador para um dos aplicativos ou serviços clusterizados e clique em **Propriedades**.

14. Na guia **Segurança**, verifique se a conta de nome do cluster está listada entre as contas que têm permissões e selecione-a. Verifique se a conta de nome do cluster possui a permissão **Controle Total** (a caixa de seleção **Permitir** é marcada). Se não tiver, adicione a conta de nome do cluster à lista e forneça a ela a permissão **Controle Total**.
    
   ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. Repita as etapas 13 e 14 para cada aplicativo ou serviço clusterizado configurado no cluster.

16. Verifique se a cota de todo o domínio para criação de objetos de computador (por padrão, 10) não foi atingida (consultando um administrador de domínio, se possível). Se todos os itens anteriores neste procedimento tiverem sido revisados e corrigidos, e se a cota tiver sido atingida, pense na possibilidade de aumenta a cota. Para alterar a cota:
    
   1.  Abra um prompt de comando como administrador e execute **ADSIEdit.msc**.  
          
   2.  Clique com o botão direito do mouse em **ADSI Edit**, clique em **Conectar-se a** e em **OK**. O **Contexto de nomenclatura padrão** é adicionado à árvore de console.  
          
   3.  Clique duas vezes em **Contexto de nomenclatura padrão**, clique com o botão direito do mouse no objeto de domínio abaixo dele e clique em **Propriedades**.  
          
   4.  Role até **ms-DS-MachineAccountQuota**, selecione-o, clique em **Editar**, altere o valor e clique em **OK**.

