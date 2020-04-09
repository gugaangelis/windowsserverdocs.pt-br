---
title: Pré-configurar objetos de computador do cluster no Active Directory Domain Services
description: Como pré-configurar objetos de computador de cluster no Active Directory Domain Services.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.openlocfilehash: b14561a05778ed30e71363a2cd3b3b6fdf24f78e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827469"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>Pré-configurar objetos de computador do cluster no Active Directory Domain Services

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico mostra como pré-configurar os objetos de computador do cluster em AD DS (Serviços de Domínio Active Directory). É possível usar esse procedimento para habilitar um usuário ou grupo para a criação de um cluster de failover quando não tiverem permissões para a criação de objetos de computador em AD DS.

Ao criar um cluster de failover usando o Assistente de Criação de Cluster ou usando o Windows PowerShell, é necessário especificar um nome para o cluster. Se tiver permissões suficientes ao criar o cluster, o processo de criação do cluster criará automaticamente um objeto de computador em AD DS que corresponda ao nome do cluster. Tal objeto é chamado de *objeto do nome do cluster* ou CNO. Por meio do CNO, os VCOs (Objetos de Computador Virtual) são automaticamente criados durante a configuração das funções clusterizadas que utilizam pontos de acesso de cliente. Por exemplo, se você criar um servidor de arquivos altamente disponível com um ponto de acesso de cliente denominado *FileServer1*, o CNO criará um VCO correspondente em AD DS.

>[!NOTE]
>Há a opção de criar um cluster desanexado Active Directory, onde nenhum CNO ou VCOs são criados no AD DS. Isso destina-se a tipos específicos de implantações de cluster. Para obter mais informações, consulte [Implantar um Cluster Desanexado do Active Directory](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>).

Para criar o CNO automaticamente, o usuário que cria o cluster de failover deve ter permissão para **Criar objetos de computador** para a OU (Unidade Organizacional) ou o contêiner onde ficam os servidores que formarão o cluster. Para habilitar um usuário ou grupo para criar um cluster sem ter essa permissão, um usuário com as permissões adequadas em AD DS (normalmente, um administrador de domínio) poderá pré-configurar o CNO no AD DS. Isso também dá ao administrador do domínio maior controle sobre a convenção de nomenclatura que é utilizada para o cluster e controle sobre qual OU na qual os objetos de cluster são criados.

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>Etapa 1: pré-configurar o CNO em AD DS

Antes de começar, certifique-se de que você tenha as seguintes informações:

- O nome que você deseja atribuir ao cluster
- O nome da conta de usuário ou grupo ao qual você deseja conceder direitos para criar o cluster

Como melhor prática, recomendamos a criação de uma OU para os objetos de cluster. Se já existir a OU pretendida, a associação no grupo **Operadores da Conta** é o requisito mínimo para concluir essa etapa. Se desejar criar uma OU para os objetos de cluster, a associação no grupo **Administradores de Domínio** ou equivalente é o requisito mínimo para concluir essa etapa.

>[!NOTE]
>Se criar o CNO no contêiner padrão dos Computadores em vez de uma OU, não será necessário concluir a Etapa 3 deste tópico. Neste cenário, um administrador de cluster pode criar até 10 VCOs sem qualquer configuração adicional.

### <a name="prestage-the-cno-in-ad-ds"></a>Pré-configurar o CNO no AD DS

1. Em um computador que tem as Ferramentas de AD DS instaladas a partir das Ferramentas de Administração de Servidor Remoto ou em um controlador de domínio, abra a opção **Usuários e Computadores do Active Directory**. Para fazer isso em um servidor, inicie o Gerenciador do Servidor e, no menu **ferramentas** , selecione **Active Directory usuários e computadores**.
2. Para criar uma UO para os objetos de computador do cluster, clique com o botão direito do mouse no nome de domínio ou em uma UO existente, aponte para **novo**e, em seguida, selecione **unidade organizacional**. Na caixa **nome** , digite o nome da UO e, em seguida, selecione **OK**.
3. Na árvore de console, clique com o botão direito do mouse na UO em que você deseja criar o CNO, aponte para **novo**e, em seguida, selecione **computador**.
4. Na caixa **nome do computador** , digite o nome que será usado para o cluster de failover e selecione **OK**.

   >[!NOTE]
   >Este será o nome do cluster que o usuário que o cria especificará na página **Ponto de Acesso para Administrar o Cluster** no Assistente de Criação de Cluster ou como o valor do parâmetro *–Name* para o cmdlet do Windows PowerShell **New-Cluster** .

5. Como prática recomendada, clique com o botão direito do mouse na conta de computador que você acabou de criar, selecione **Propriedades**e, em seguida, selecione a guia **objeto** . Na guia **objeto** , marque a caixa de seleção **proteger objeto contra exclusão acidental** e selecione **OK**.
6. Clique com o botão direito do mouse na conta de computador que você acabou de criar e selecione **desabilitar conta**. Selecione **Sim** para confirmar e, em seguida, selecione **OK**.

   >[!NOTE]
   >É necessário desabilitar a conta, de modo que durante a criação do cluster, o processo de criação do cluster possa confirmar que a conta não está sendo usada no momento por um computador ou cluster existentes no domínio.

![CNO desabilitado no exemplo da OU dos Clusters](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**Figura 1. O CNO foi desabilitado na UO de clusters de exemplo**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>Etapa 2: conceder permissões para o usuário para criar o cluster

Você deve configurar permissões para que a conta de usuário que será usada para criar o cluster de failover tenha permissões de Controle Total em relação ao CNO.

A associação ao grupo **Operadores de Conta** é o mínimo necessário para concluir esta etapa.

Veja como conceder as permissões de usuário para criar o cluster:

1. Em Usuários e Computadores do Active Directory, no menu **Exibir**, verifique se a opção **Recursos Avançados** está selecionada.
2. Localize, clique com o botão direito do mouse no CNO e selecione **Propriedades**.
3. Na guia **segurança** , selecione **Adicionar**.
4. Na caixa de diálogo **Selecionar usuários, computadores ou grupos** , especifique a conta de usuário ou o grupo ao qual você deseja conceder permissões e, em seguida, selecione **OK**.
5. Selecione a conta de usuário ou grupo que acabou de adicionar e, em seguida, ao lado de **Controle total**, marque a caixa de seleção **Permitir**.
  
   ![Concedendo Controle Total ao usuário ou grupo que criará o cluster](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
   **Figura 2. Concedendo controle total ao usuário ou grupo que criará o cluster**
6. Selecione **OK**.

Após concluir essa etapa, o usuário para o qual você concedeu as permissões poderá criar o cluster de failover. No entanto, se o CNO estiver localizado em uma OU, o usuário não poderá criar funções clusterizadas que exijam um ponto de acesso de cliente até que você conclua a Etapa 3.

>[!NOTE]
>Se o CNO estiver no contêiner de Computadores padrão, um administrador de cluster poderá criar até 10 VCOs sem qualquer configuração adicional. Para adicionar mais de 10 VCOs, você terá que conceder permissão explícita para **Criar objetos de computador** ao CNO do contêiner de computadores.

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>Etapa 3: conceder as permissões de CNO para a OU ou pré-configurar VCOs das funções clusterizadas

Ao criar uma função clusterizada com um ponto de acesso cliente, o cluster cria um VCO na mesma OU do CNO. Para que isso ocorra de forma automática, o CNO deve ter as permissões para criar objetos de computador na OU.

Caso tenha pré-configurado o CNO em AD DS, será possível escolher uma das opções abaixo para criar os VCOs:

- Opção 1: [Conceder as permissões do CNO à OU](#grant-the-cno-permissions-to-the-ou). Se usar essa opção, o cluster poderá criar os VCOs automaticamente em AD DS. Portanto, um administrador de cluster de failover poderá criar funções clusterizadas sem ter que solicitar que você pré-configure os VCOs em AD DS.

>[!NOTE]
>A associação ao grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir as etapas desta opção.

- Opção 2: [pré-configurar um VCO para uma função clusterizada](#prestage-a-vco-for-a-clustered-role). Utilize essa opção se for necessário pré-configurar as contas das funções clusterizadas devido aos requisitos de sua organização. Por exemplo, talvez você queira controlar a convenção de nomenclatura ou controlar quais funções clusterizadas são criadas.

>[!NOTE]
>A associação ao grupo **Operadores da Conta** é o mínimo necessário para concluir as etapas desta opção.

### <a name="grant-the-cno-permissions-to-the-ou"></a>Conceder as permissões do CNO para a UO

1. Em Usuários e Computadores do Active Directory, no menu **Exibir**, verifique se a opção **Recursos Avançados** está selecionada.
2. Clique com o botão direito do mouse na UO em que você criou o CNO na [etapa 1: pré-teste o CNO em AD DS](#step-1-prestage-the-cno-in-ad-ds)e, em seguida, selecione **Propriedades**.
3. Na guia **segurança** , selecione **avançado**.
4. Na caixa de diálogo **configurações de segurança avançadas** , selecione **Adicionar**.
5. Ao lado de **principal**, selecione **selecionar uma entidade de segurança**.
6. Na caixa de diálogo **Selecionar usuário, computador, conta de serviço ou grupos** , selecione **tipos de objeto**, marque a caixa de seleção **computadores** e, em seguida, selecione **OK**.
7. Em **Inserir os nomes de objeto a serem selecionados**, insira o nome do CNO, selecione **verificar nomes**e, em seguida, selecione **OK**. Em resposta à mensagem de aviso que diz que você está prestes a adicionar um objeto desabilitado, selecione **OK**.
8. Na caixa de diálogo **Entrada de Permissão**, verifique se a lista **Tipo** está configurada para **Permitir** e se a lista **Aplicável a** está configurada para **Este objeto e todos os objetos descendentes**.
9. Em **Permissões**, marque a caixa de seleção **Criar objetos de computador**.

   ![Concedendo a permissão para Criar objetos de computador ao CNO](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

   **Figura 3. Concedendo a permissão CREATE Computer Objects ao CNO**
10. Selecione **OK** até retornar ao snap-in Active Directory usuários e computadores.

Um administrador no cluster de failover poderá agora criar funções clusterizadas com pontos de acesso de cliente e colocar os recursos online.

### <a name="prestage-a-vco-for-a-clustered-role"></a>Pré-testar um VCO para uma função clusterizada

1. Antes de começar, você precisa saber o nome do cluster e o nome que a função clusterizada terá.
2. Em Usuários e Computadores do Active Directory, no menu **Exibir**, verifique se a opção **Recursos Avançados** está selecionada.
3. Em Active Directory usuários e computadores, clique com o botão direito do mouse na UO em que o CNO para o cluster reside, aponte para **novo**e, em seguida, selecione **computador**.
4. Na caixa **nome do computador** , digite o nome que será usado para a função clusterizada e selecione **OK**.
5. Como prática recomendada, clique com o botão direito do mouse na conta de computador que você acabou de criar, selecione **Propriedades**e, em seguida, selecione a guia **objeto** . Na guia **objeto** , marque a caixa de seleção **proteger objeto contra exclusão acidental** e selecione **OK**.
6. Clique com o botão direito do mouse na conta de computador que você acabou de criar e selecione **Propriedades**.
7. Na guia **segurança** , selecione **Adicionar**.
8. Na caixa de diálogo **Selecionar usuário, computador, conta de serviço ou grupos** , selecione **tipos de objeto**, marque a caixa de seleção **computadores** e, em seguida, selecione **OK**.
9. Em **Inserir os nomes de objeto a serem selecionados**, insira o nome do CNO, selecione **verificar nomes**e, em seguida, selecione **OK**. Se você receber uma mensagem de aviso dizendo que está prestes a adicionar um objeto desabilitado, selecione **OK**.
10. Verifique se o CNO está selecionado e, em seguida, ao lado de **Controle total**, marque a caixa de seleção **Permitir**.
11. Selecione **OK**.

Um administrador no cluster de failover poderá agora criar funções clusterizadas com um ponto de acesso de cliente que corresponda ao nome do VCO pré-configurado e colocar os recursos online.

## <a name="more-information"></a>Mais informações

- [Clustering de failover](failover-clustering.md)
- [Configuração de contas de cluster no Active Directory](configure-ad-accounts.md)