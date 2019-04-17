---
title: Inserir objetos de computador do cluster nos serviços de domínio Active Directory
description: Como inserir objetos de computador do cluster nos serviços de domínio Active Directory.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/25/2018
ms.localizationpriority: medium
ms.openlocfilehash: 111969b074b33764dbbf72bfb24ad606f8314e41
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081782"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>Inserir objetos de computador do cluster nos serviços de domínio Active Directory

>Aplica-se a: Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2016

Este tópico mostra como inserir objetos de computador do cluster nos serviços de domínio Active Directory (AD DS). Você pode usar este procedimento para habilitar um usuário ou grupo criar um cluster de failover, quando eles não tem permissões para criar objetos de computador no AD DS.

Quando você cria um cluster de failover usando o Assistente para criação de Cluster ou usando o Windows PowerShell, você deve especificar um nome para o cluster. Se você tiver permissões suficientes ao criar o cluster, o processo de criação de cluster cria automaticamente um objeto de computador no AD DS que corresponde ao nome do cluster. Este objeto é chamado, o *objeto de nome de cluster* ou o CNO. O CNO, por meio de objetos de computador virtual (VCOs) são criados automaticamente quando você configura clusterizadas funções que usam os pontos de acesso de cliente. Por exemplo, se você criar um servidor de arquivos altamente disponível com um ponto de acesso de cliente nomeado *FileServer1*, o CNO criará um VCO correspondente no AD DS.

>[!NOTE]
>No Windows Server 2012 R2, há a opção para criar um cluster desanexado o Active Directory, onde nenhuma CNO ou VCOs são criados no AD DS. Isso é direcionado para tipos específicos de implantações do cluster. Para obter mais informações, consulte [Deploy um Cluster Active Directory-Detached](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>).

Para criar o CNO automaticamente, o usuário que cria o cluster de failover deve ter a permissão de **objetos de computador criar** para a unidade organizacional (UO) ou o contêiner onde residem os servidores que serão formar um cluster. Para habilitar um usuário ou grupo criar um cluster sem ter esta permissão, um usuário com as permissões apropriadas no AD DS (normalmente um administrador de domínio) pode inserir o CNO no AD DS. Isso também fornece ao administrador de domínio mais controle sobre a convenção de nomenclatura que é usado para o cluster e controle sobre qual UO os objetos de cluster são criados no.

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>Etapa 1: Inserir o CNO no AD DS

Antes de começar, certifique-se de que você sabe o seguinte:

- O nome que você deseja atribuir ao cluster
- O nome da conta de usuário ou grupo ao qual você deseja conceder direitos para criar o cluster

Como prática recomendada, recomendamos que você crie uma unidade Organizacional para os objetos de cluster. Se uma UO já existir que você deseja usar, a associação ao grupo **Operadores** de conta é o mínimo necessário para concluir esta etapa. Se você precisar criar uma unidade Organizacional para os objetos de cluster, a associação no grupo **Admins de domínio** , ou equivalente, é o mínimo necessário para concluir esta etapa.

>[!NOTE]
>Se você criar o CNO no contêiner de computadores padrão, em vez de uma unidade Organizacional, você não precisará concluir a etapa 3 deste tópico. Neste cenário, um administrador de cluster pode criar até 10 VCOs sem nenhuma configuração adicional.

### <a name="prestage-the-cno-in-ad-ds"></a>Inserir o CNO no AD DS

1. Em um computador que tenha as ferramentas do AD DS instalado a partir das ferramentas de administração de servidor remoto, ou em um controlador de domínio, abra **e computadores do Active Directory Users**. Para fazer isso em um servidor, inicie o Gerenciador do servidor e, em seguida, no menu **Ferramentas** , selecione **e computadores do Active Directory Users**.
2. Para criar objetos de computador de uma unidade Organizacional para o cluster, clique com o botão o nome de domínio ou uma UO existente, aponte para **novo**e selecione **Unidade organizacional**. Na caixa **nome** , digite o nome da unidade Organizacional e, em seguida, selecione **Okey**.
3. Na árvore de console, clique com o botão UO onde você deseja criar o CNO, aponte para **novo**e depois selecione **computador**.
4. Na caixa **nome do computador** , insira o nome que será usado para o cluster de failover e selecione **Okey**.

  >[!NOTE]
  >Este é o nome do cluster que o usuário que cria o cluster especificará na página do **Ponto de acesso para administrar o Cluster** no Assistente para criar o Cluster ou como o valor do *– nome* parâmetro para o **Cluster do novo** Windows PowerShell cmdlet.

5. Como prática recomendada, com o botão direito a conta de computador que você acabou de criar, selecione **Propriedades**e, em seguida, selecione a guia de **objeto** . Na guia **objeto** , marque a caixa de seleção **proteger objeto contra exclusão acidental** e selecione **Okey**.
6. Clique com o botão a conta de computador que você acabou de criar e selecione **Desativar conta**. Selecione **Sim** para confirmar e, em seguida, selecione **Okey**.

  >[!NOTE]
  >Você deve desabilitar a conta para que durante a criação do cluster, o processo de criação de cluster possa confirmar que a conta não esteja sendo usado por um computador existente ou cluster no domínio.

![CNO desabilitado no exemplo Clusters de UO](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**Figura 1. CNO desabilitado no exemplo Clusters de UO**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>Etapa 2: Conceder as permissões de usuário para criar o cluster

Você deve configurar permissões para que a conta de usuário que será usada para criar o cluster de failover tem permissões de controle total para o CNO.

A associação ao grupo **Operadores** de conta é o mínimo necessário para concluir esta etapa.

Aqui está como conceder as permissões de usuário para criar o cluster:

1. No Active Directory Users and Computers, no menu **Exibir** , certifique-se de que os **Recursos avançados** está selecionado.
2. Localize e, em seguida, clique com o botão o CNO e selecione **Propriedades**.
3. Na guia **segurança** , selecione **Adicionar**.
4. Na caixa de diálogo **Selecionar usuários, computadores ou grupos** , especifique a conta de usuário ou grupo que você deseja conceder permissões para e selecione **Okey**.
5. Selecione a conta de usuário ou grupo que você acabou de adicionar e, em seguida, ao lado de **controle total**, selecione a caixa de seleção **Permitir** .
  
  ![Concedendo controle total para o usuário ou grupo que criará o cluster](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **Figura 2. Concedendo controle total para o usuário ou grupo que criará o cluster**
6. Selecione **OK**.

Após concluir esta etapa, o usuário que você concedeu permissões para pode criar o cluster de failover. No entanto, se o CNO estiver localizado em uma unidade Organizacional, o usuário não pode criar funções clusterizadas que exigem um ponto de acesso do cliente até que você conclua a etapa 3.

>[!NOTE]
>Se o CNO estiver no contêiner de computadores padrão, um administrador de cluster pode criar até 10 VCOs sem nenhuma configuração adicional. Para adicionar mais de 10 VCOs, você deve conceder explicitamente a permissão **criar computador objetos** ao CNO para o contêiner de computadores.

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>Etapa 3: Conceda as permissões de CNO a unidade Organizacional ou inserir VCOs para funções em cluster

Quando você cria uma função agrupada com um ponto de acesso do cliente, o cluster cria um VCO na mesma UO como o CNO. Para que isso ocorra automaticamente, o CNO deve ter permissões para criar objetos de computador na unidade Organizacional.

Se você tiver inserido o CNO no AD DS, você pode fazer um dos procedimentos a seguir para criar VCOs:

- Opção 1: [conceder as permissões de CNO à unidade Organizacional](#grant-the-cno-permissions-to-the-ou). Se você usar essa opção, o cluster pode criar automaticamente VCOs no AD DS. Portanto, um administrador para o cluster de failover pode criar funções clusterizadas sem ter que pedir que você insere VCOs no AD DS.

>[!NOTE]
>A associação no grupo de **Administradores de domínio** , ou equivalente, é o mínimo necessário para concluir as etapas para essa opção.

- Opção 2: [Prestage um VCO para uma função agrupado](#prestage-a-vco-for-the-clustered-role). Use esta opção se for necessário inserir contas para funções agrupadas por causa de requisitos em sua organização. Por exemplo, convém controlar a convenção de nomenclatura, ou o controle de quais funções agrupadas são criadas.

>[!NOTE]
>A associação ao grupo **Operadores** de conta é o mínimo necessário para concluir as etapas para essa opção.

### <a name="grant-the-cno-permissions-to-the-ou"></a>Conceda as permissões de CNO a unidade Organizacional

1. No Active Directory Users and Computers, no menu **Exibir** , certifique-se de que os **Recursos avançados** está selecionado.
2. Com o botão direito em que você criou o CNO na unidade Organizacional [etapa 1: inserir o CNO no AD DS](#step-1:-prestage-the-CNO-in-ad-ds)e selecione **Propriedades**.
3. Na guia **segurança** , selecione **Avançado**.
4. Na caixa de diálogo **Configurações avançadas de segurança** , selecione **Adicionar**.
5. Ao lado de **entidade de segurança**, selecione **Selecionar uma entidade**.
6. Na caixa de diálogo **Selecionar usuário, computador, a conta de serviço ou grupos** , selecione **Os tipos de objeto**, marque a caixa de seleção de **computadores** e selecione **Okey**.
7. Sob **Insira os nomes de objeto a serem selecionados**, digite o nome do CNO, selecione **Verificar nomes**e, em seguida, selecione **Okey**. Em resposta à mensagem de aviso que diz que você está tentando adicionar um objeto desativado, selecione **Okey**.
8. Na caixa de diálogo **Entrada de permissão** , certifique-se de que a lista de **tipo** está definida como **Permitir**e **aplica-se a** lista está definida para **Este objeto e todos os objetos descendentes**.
9. Em **permissões**, marque a caixa de seleção de **objetos de computador criar** .

  ![Conceder a permissão de objetos de computador criar ao CNO](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **Figura 3. Conceder a permissão de objetos de computador criar ao CNO**
10. Selecione **Okey** até você retornar para os usuários do Active Directory e o snap-in de computadores.

Um administrador no cluster de failover agora pode criar funções agrupadas com cliente pontos de acesso e colocar os recursos online.

### <a name="prestage-a-vco-for-a-clustered-role"></a>Inserir um VCO para uma função em cluster

1. Antes de começar, certifique-se de que você sabe o nome do cluster e o nome que a função clusterizada terá.
2. No Active Directory Users and Computers, no menu **Exibir** , certifique-se de que os **Recursos avançados** está selecionado.
3. Em e computadores do Active Directory Users, do mouse em unidade Organizacional em que reside o CNO para o cluster, aponte para **novo**e depois selecione **computador**.
4. Na caixa **nome do computador** , digite o nome que você usará para a função de cluster e selecione **Okey**.
5. Como prática recomendada, com o botão direito a conta de computador que você acabou de criar, selecione **Propriedades**e, em seguida, selecione a guia de **objeto** . Na guia **objeto** , marque a caixa de seleção **proteger objeto contra exclusão acidental** e selecione **Okey**.
6. Clique com o botão a conta de computador que você acabou de criar e selecione **Propriedades**.
7. Na guia **segurança** , selecione **Adicionar**.
8. Na caixa de diálogo **Selecionar usuário, computador, a conta de serviço ou grupos** , selecione **Os tipos de objeto**, marque a caixa de seleção de **computadores** e selecione **Okey**.
9. Sob **Insira os nomes de objeto a serem selecionados**, digite o nome do CNO, selecione **Verificar nomes**e, em seguida, selecione **Okey**. Se você receber uma mensagem de aviso que diz que você está tentando adicionar um objeto desativado, selecione **Okey**.
10. Certifique-se de que o CNO esteja selecionado e, em seguida, ao lado de **controle total**, selecione a caixa de seleção **Permitir** .
11. Selecione **OK**.

Um administrador no cluster de failover agora pode criar a função agrupada com um ponto de acesso do cliente que corresponde ao nome VCO inserido e coloque o recurso online.

## <a name="more-information"></a>Mais informações

- [Clustering de failover](failover-clustering.md)