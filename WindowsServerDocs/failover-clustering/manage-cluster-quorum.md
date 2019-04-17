---
title: Configurar e gerenciar o quorum em um cluster de failover
description: Obter informações detalhadas sobre como gerenciar o quorum de cluster em um cluster de failover do Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 05/01/2018
ms.localizationpriority: medium
ms.openlocfilehash: cf7dbf7d92bf8abf3d94910ceaaf5520bf7a0447
ms.sourcegitcommit: dfd25348ea3587e09ea8c2224237a3e8078422ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4678635"
---
# Configurar e gerenciar o quorum

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece as etapas para configurar e gerenciar o quorum em um cluster de failover do Windows Server e plano de fundo.

## Quorum de compreensão

O quorum para um cluster é determinado pelo número de elementos voto que deve ser parte da associação de cluster ativo para que o cluster iniciado corretamente ou continuar em execução. Para uma explicação mais detalhada, consulte as [Noções básicas sobre o documento de quorum de cluster e pool](../storage/storage-spaces/understand-quorum.md).

## Opções de configuração de quorum

O modelo de quorum no Windows Server é flexível. Se você precisar modificar a configuração de quorum de cluster, você pode usar o Assistente para Configurar Quorum de Cluster ou os cmdlets do PowerShell de Windows de Clusters de Failover. Para as etapas e considerações para configurar o quorum, consulte [Configurar o quórum do cluster](#configure-the-cluster-quorum) neste tópico.

A tabela a seguir lista as opções de configuração de três quorum que estão disponíveis no Assistente para Configurar Quorum de Cluster.

<table>
<thead>
<tr class="header">
<th>Opção</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Usar as configurações típicas</td>
<td>Cluster atribui automaticamente um voto em cada nó e gerencia dinamicamente os votos de nó. Se ele é adequado para o cluster e armazenamento de cluster compartilhado está disponível, o cluster seleciona uma testemunha de disco. Essa opção é recomendada na maioria dos casos, porque o software do cluster escolhe automaticamente uma configuração de quorum e testemunha que fornece a mais alta disponibilidade para o cluster.</td>
</tr>
<tr class="even">
<td>Adicionar ou alterar a testemunha de quorum</td>
<td>Você pode adicionar, alterar ou remover um recurso de testemunha. Você pode configurar uma testemunha de compartilhamento ou disco. Cluster atribui automaticamente um voto em cada nó e gerencia dinamicamente os votos de nó.</td>
</tr>
<tr class="odd">
<td>A configuração de quorum avançados e seleção de testemunha</td>
<td>Você deve selecionar essa opção somente quando você tiver requisitos específicos do aplicativo ou específicos do site para configurar o quorum. Você pode modificar a testemunha de quorum, adicionar ou remover votos de nó e escolha se o cluster gerencia dinamicamente votos de nó. Por padrão, votos são atribuídos a todos os nós e os votos de nó dinamicamente são gerenciados.</td>
</tr>
</tbody>
</table>

Dependendo da opção de configuração de quorum que você escolher e suas configurações específicas, o cluster será configurado em um dos seguintes modos de quorum:

<table>
<thead>
<tr class="header">
<th>Modo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Maioria de nós (nenhuma testemunha)</td>
<td>Somente os nós ter votos. Nenhuma testemunha de quorum está configurada. O quórum do cluster é a maioria de nós voto na associação de cluster ativo.</td>
</tr>
<tr class="even">
<td>Maioria de nós com testemunha (disco ou compartilhamento de arquivos)</td>
<td>Nós têm votos. Além disso, uma testemunha de quorum tem um voto. O quórum do cluster é a maioria de nós voto na associação do cluster ativo mais um voto testemunha.<br />
<br />
Uma testemunha de quorum pode ser uma testemunha de disco designado ou uma testemunha de compartilhamento de arquivo designado.</td>
</tr>
<tr class="odd">
<td>Sem maioria (somente testemunha de disco)</td>
<td>Nenhum nó têm votos. Apenas uma testemunha de disco tem um voto. O quórum do cluster é determinado pelo estado de testemunha de disco.<br />
<br />
O cluster tem quorum se um nó está disponível e estão se comunicando com um disco específico do armazenamento de cluster. Em geral, esse modo não é recomendado e ele não deve ser selecionado porque ele cria um único ponto de falha para o cluster.</td>
</tr>
</tbody>
</table>

As seguintes subseções dará obter mais informações sobre as definições de configuração de quorum avançados.

### Configuração de testemunha

Como regra geral quando você configura um quorum, os elementos voto no cluster devem ser um número ímpar. Portanto, se o cluster contém um número par de voto nós, você deve configurar uma testemunha de disco ou uma testemunha de compartilhamento de arquivos. O cluster será capaz de oferecer um nó adicional para baixo. Além disso, adicionar um voto testemunha permite que o cluster continue executando se metade de nós de cluster simultaneamente descer ou estiver desconectado.

Uma testemunha de disco geralmente é recomendada se todos os nós podem ver o disco. Uma testemunha de compartilhamento de arquivos é recomendada quando você precisa considerar a recuperação de desastres multissite com armazenamento replicado. Configurar uma testemunha de disco com armazenamento replicado é possível somente se o fornecedor de armazenamento oferece suporte ao acesso de leitura / gravação em todos os sites no armazenamento replicado. <strong>*Uma testemunha de disco não é compatível com espaços de armazenamento diretos*</strong>.

A tabela a seguir fornece informações adicionais e considerações sobre os tipos de testemunha de quorum.

<table>
<thead>
<tr class="header">
<th>Tipo de testemunha</th>
<th>Descrição</th>
<th>Requisitos e recomendações</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Testemunha de disco</td>
<td>- LUNs dedicado que armazena uma cópia do banco de dados do cluster<br />
- Mais útil para clusters com armazenamento compartilhado (não replicado)</td>
<td>- Tamanho do LUNs deve ser pelo menos 512 MB<br />
- Deve ser dedicada para uso do cluster e não atribuídos a uma função clusterizada<br />
- Devem ser incluídos em testes de validação de armazenamento em cluster de armazenamento e passe<br />
- Não pode ser um disco que é um Volume compartilhado do Cluster (CSV)<br />
- Disco básico com um único volume<br />
- Não precisa ter uma letra de unidade<br />
- Pode ser formatada com NTFS ou ReFS<br />
- Pode ser configurado opcionalmente com RAID de hardware para tolerância a falhas<br />
- Devem ser excluídos do backups e verificação antivírus<br />
- Uma testemunha de disco não é compatível com espaços de armazenamento diretos</td>
</tr>
<tr class="even">
<td>Testemunha de compartilhamento de arquivos</td>
<td>- Compartilhamento de arquivos SMB que está configurado em um servidor de arquivos que executam o Windows Server<br />
- Não armazena uma cópia do banco de dados do cluster<br />
- Mantém as informações de cluster somente em um arquivo de witness.log<br />
- Mais útil para clusters multissite com armazenamento replicado</td>
<td>- Deve ter pelo menos 5 MB de espaço livre<br />
- Deve ser dedicada ao cluster único e não são usados para armazenar dados de usuário ou aplicativo<br />
- Devem ter permissões de gravação habilitado para o objeto de computador para o nome do cluster<br />
<br />
Considerações adicionais para um servidor de arquivos que hospeda a testemunha de compartilhamento de arquivos são:<br />
<br />
- Um único servidor de arquivos pode ser configurado com testemunhas de compartilhamento de arquivo para vários clusters.<br />
- O servidor de arquivos deve estar em um site que é separado da carga de trabalho da cluster. Isso permite a oportunidade de igual para qualquer site de cluster sobreviver se comunicação de rede to-site for perdida. Se o servidor de arquivos estiver no mesmo site, esse site se torna o site principal, e é o site único que pode acessar o compartilhamento de arquivos.<br />
- O servidor de arquivos pode executar em uma máquina virtual se a máquina virtual não está hospedada no mesmo cluster que usa a testemunha de compartilhamento de arquivos.<br />
- Alta disponibilidade, o servidor de arquivos pode ser definido em um cluster de failover separado.</td>
</tr>
</tbody>
</table>

### Atribuição de nó de voto

Como uma opção de configuração de quorum avançados, você pode optar por atribuir ou remover votos de quorum em uma base por nó. Por padrão, todos os nós são atribuídos votos. Independentemente da atribuição de voto, todos os nós continuam a funcionar no cluster, receberá atualizações de banco de dados do cluster e podem hospedar aplicativos.

Você talvez queira remover votos de nós em determinadas configurações de recuperação de desastres. Por exemplo, em um cluster multissite, você pode remover votos de nós em um site de backup para que os nós não afetam os cálculos de quorum. Essa configuração é recomendada apenas para failover manual entre sites. Para obter mais informações, consulte [as considerações de Quorum para configurações de recuperação de desastres](#quorum-considerations-for-disaster-recovery-configurations) posteriormente neste tópico.

O voto configurado de um nó pode ser verificado ao consultar a propriedade comum **NodeWeight** do nó de cluster usando o cmdlet [Get-ClusterNode](http://technet.microsoft.com/library/hh847268.aspx)do Windows PowerShell. Um valor 0 indica que o nó não tenha um voto de quorum configurado. Um valor de 1 indica que o voto quorum do nó é atribuído, e ele é gerenciado pelo cluster. Para obter mais informações sobre o gerenciamento de nó votos, consulte [gerenciamento de quorum dinâmicos](#dynamic-quorum-management) neste tópico.

A atribuição de voto para todos os nós de cluster pode ser verificada usando o teste de validação de **Validar quórum do Cluster** .

#### Considerações adicionais sobre o nó votar atribuição

  - Atribuição de nó de voto não é recomendada para impor um número ímpar de nós voto. Em vez disso, você deve configurar uma testemunha de disco ou testemunha de compartilhamento de arquivos. Para obter mais informações, consulte [configuração de testemunha](#witness-configuration) posteriormente neste tópico.
  - Se o gerenciamento de quorum dinâmico é ativado, somente os nós que estão configurados para ter votos de nó atribuídos podem ter seus votos atribuídos ou removidos dinamicamente. Para obter mais informações, consulte [gerenciamento de quorum dinâmicos](#dynamic-quorum-management) neste tópico.

### Gerenciamento de quorum dinâmico

No Windows Server 2012, como uma opção de configuração de quorum avançados, você pode optar por habilitar o gerenciamento de quorum dinâmico por cluster. Para obter mais detalhes sobre como dinâmico funciona de quorum, consulte [essa explicação](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

Com o gerenciamento de quorum dinâmico, também é possível para um cluster seja executado no último nó do cluster sobreviventes. Ajustando dinamicamente o requisito de maioria de quorum, um cluster pode suportar desligamentos sequencial nó para um único nó.

O voto dinâmico atribuída pelo cluster de um nó pode ser verificado com a propriedade comum **DynamicWeight** do nó de cluster usando o cmdlet [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) do Windows PowerShell. Um valor 0 indica que o nó não tenha um voto de quorum. Um valor de 1 indica que o nó tem um voto de quorum.

A atribuição de voto para todos os nós de cluster pode ser verificada usando o teste de validação de **Validar quórum do Cluster** .

#### Considerações adicionais sobre o gerenciamento de quorum dinâmico

- Gerenciamento de quorum dinâmico não permitir que o cluster sustentar uma falha simultânea da maioria dos membros voto. Para continuar em execução, o cluster deve sempre ter a maioria de quorum no momento do nó desligamento ou falha.

- Se você removeu explicitamente o voto de um nó, o cluster dinamicamente não é possível adicionar ou remover esse voto.
- Quando espaços de armazenamento diretos está habilitado, o cluster só pode suportar duas falhas do nó. Isso é explicado mais na [seção de quorum de pool](../storage/storage-spaces/understand-quorum.md#poolQuorum)

## Recomendações gerais para a configuração de quorum

O software do cluster configura automaticamente o quorum em um novo cluster, com base no número de nós configurados e a disponibilidade de armazenamento compartilhado. Isso geralmente é a configuração de quorum mais apropriada para esse cluster. No entanto, é uma boa ideia para examinar a configuração de quorum após a criação do cluster, antes de colocar o cluster em produção. Para exibir a configuração de quorum de cluster detalhados, você pode usar o validar um Assistente de configuração ou o cmdlet [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) Windows PowerShell, para executar o teste de **Validar a configuração de Quorum** . No Gerenciador de Cluster de Failover, a configuração de quorum básico é exibida nas informações de resumo para o cluster selecionado, ou você pode revisar as informações sobre recursos de quorum que retorna ao executar o PowerShell [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) cmdlet.

A qualquer momento, você pode executar o teste de **Validar a configuração de Quorum** para validar a configuração de quorum é ideal para seu cluster. A saída do teste indica se uma alteração na configuração de quorum é recomendada e as configurações que sejam ideais. Se uma alteração é recomendada, você pode usar o Assistente para Configurar Quorum de Cluster para aplicar as configurações recomendadas.

Depois que o cluster estiver em produção, não altere a configuração de quorum, a menos que você determinou que a alteração é apropriada para seu cluster. Convém considerar a alteração da configuração de quorum nas seguintes situações:

- Adicionando ou removendo nós
- Adicionando ou removendo armazenamento
- Uma falha de nó ou testemunha de longo prazo
- Recuperando um cluster em um cenário de recuperação de desastres multissite

Para obter mais informações sobre como validar um cluster de failover, consulte [Validar o Hardware para um Cluster de Failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## Configurar o quórum do cluster

Você pode configurar as configurações de quorum de cluster usando o Gerenciador de Cluster de Failover ou os cmdlets do PowerShell de Windows de Clusters de Failover.

>[!IMPORTANT]
>É melhor usar a configuração de quorum recomendado pelo Assistente para Configurar Quorum de Cluster. É recomendável Personalizando a configuração de quorum somente se você determinou que a alteração é apropriada para seu cluster. Para obter mais informações, consulte [as recomendações gerais para a configuração de quorum](#general-recommendations-for-quorum-configuration) neste tópico.

### Defina as configurações de quorum de cluster

Participação no grupo de **administradores** local em cada servidor de cluster, ou equivalente, é as permissões mínimas necessárias para concluir este procedimento. Além disso, a conta usada deve ser uma conta de usuário de domínio.

>[!NOTE]
>Você pode alterar a configuração de quorum de cluster sem parar os cluster ou a realização de recursos de cluster offline.

### Alterar a configuração de quorum em um cluster de failover usando o Gerenciador de Cluster de Failover

1. No Gerenciador de Cluster de Failover, selecione ou especifique o cluster que você deseja alterar.
2. Com o cluster selecionado, em **ações**, selecione **Mais ações**e, em seguida, selecione **Definir configurações de Quorum de Cluster**. O Assistente para Configurar Quorum do Cluster é exibida. Selecione **Avançar**.
3. Na página **Selecionar opção de configuração de Quorum** , selecione uma das três opções de configuração e conclua as etapas para essa opção. Antes de configurar as configurações de quorum, você pode revisar suas escolhas. Para obter mais informações sobre as opções, consulte [Visão geral sobre o quorum em um cluster de failover](#overview-of-the-quorum-in-a-failover-cluster), anteriormente neste tópico.

    - Para permitir que o cluster redefinir automaticamente as configurações de quorum que são ideais para a configuração atual do cluster, selecione **usar configurações típicas** e, em seguida, conclua o assistente.
    - Para adicionar ou alterar a testemunha de quorum, selecione **Adicionar ou alterar a testemunha de quorum**e, em seguida, conclua as seguintes etapas. Para obter informações e considerações sobre como configurar uma testemunha de quorum, consulte [configuração de testemunha](#witness-configuration) anteriormente neste tópico.

      1. Na página **Selecionar testemunha de Quorum** , selecione uma opção para configurar uma testemunha de disco ou uma testemunha de compartilhamento de arquivos. O assistente indica as opções de seleção de testemunha que são recomendadas para seu cluster.

          >[!NOTE]
          >Você também pode selecionar **não configurar uma testemunha de quorum** e conclua o assistente. Se você tiver um número par de voto nós no cluster, isso não pode ser uma configuração recomendada.

      2. Se você selecionar a opção de configurar uma testemunha de disco, na página **Configurar a testemunha de armazenamento** , selecione o volume de armazenamento que você deseja atribuir como testemunha de disco e, em seguida, conclua o assistente.
      3. Se você selecionar a opção de configurar uma testemunha de compartilhamento de arquivos, na página **Configurar a testemunha de compartilhamento** , digite ou navegue para um compartilhamento de arquivo que será usado como o recurso de testemunha e conclua o assistente.

    - Para definir as configurações de gerenciamento de quorum e para adicionar ou alterar a testemunha de quorum, selecione **a configuração de quorum avançados e seleção de testemunha**e, em seguida, conclua as seguintes etapas. Para obter informações e considerações sobre as definições de configuração de quorum avançados, consulte [nó votar atribuição](#node-vote-assignment) e [gerenciamento de quorum dinâmico](#dynamic-quorum-management) anteriormente neste tópico.

      1. Na página **Select Configuration voto** , selecione uma opção para atribuir votos a nós. Por padrão, todos os nós são atribuídos um voto. No entanto, para determinados cenários, você pode atribuir votos apenas a um subconjunto de nós.

          >[!NOTE]
          >Você também pode selecionar **Não nós**. Isso geralmente não é recomendado, porque ele não permite nós para participar de quorum voto e requer configurar uma testemunha de disco. Este testemunha de disco se torna o único ponto de falha para o cluster.

      2. Na página **Configurar o gerenciamento de Quorum** , você pode habilitar ou desabilitar a opção **Permitir de cluster para gerenciar dinamicamente a atribuição de nó votos** . Selecionar essa opção geralmente aumenta a disponibilidade do cluster. Por padrão, a opção está ativada, e é altamente recomendável não desativar essa opção. Essa opção permite que o cluster continuem funcionando em cenários de falha que não são possíveis quando essa opção está desativada.
      3. Na página **Selecionar testemunha de Quorum** , selecione uma opção para configurar uma testemunha de disco ou uma testemunha de compartilhamento de arquivos. O assistente indica as opções de seleção de testemunha que são recomendadas para seu cluster.

          >[!NOTE]
          >Você pode selecionar também **não configurar uma testemunha de quorum**e conclua o assistente. Se você tiver um número par de voto nós no cluster, isso não pode ser uma configuração recomendada.

      4. Se você selecionar a opção de configurar uma testemunha de disco, na página **Configurar a testemunha de armazenamento** , selecione o volume de armazenamento que você deseja atribuir como testemunha de disco e, em seguida, conclua o assistente.
      5. Se você selecionar a opção de configurar uma testemunha de compartilhamento de arquivos, na página **Configurar a testemunha de compartilhamento** , digite ou navegue para um compartilhamento de arquivo que será usado como o recurso de testemunha e conclua o assistente.

4. Selecione **Avançar**. Confirme suas seleções na página de confirmação que aparece e selecione **Avançar**.

Depois que o assistente é executado e a página de **Resumo** é exibida, se você quiser exibir um relatório das tarefas realizadas pelo assistente, selecione **Exibir o relatório**. O relatório mais recente permanecerá na *systemroot * * * \\Cluster\\Reports** pasta com o nome **QuorumConfiguration.mht**.

>[!NOTE]
>Depois de configurar o quórum do cluster, recomendamos que você executar o teste de **Validar a configuração de Quorum** para verificar as configurações de quorum atualizados.

### Comandos equivalentes do Windows PowerShell

Os exemplos a seguir mostram como usar o cmdlet [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) e outros cmdlets do Windows PowerShell para configurar o quórum do cluster.

O exemplo a seguir altera a configuração de quorum em cluster *CONTOSO FC1* para uma configuração de maioria de nós simples com nenhuma testemunha de quorum.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

O exemplo a seguir altera a configuração de quorum no cluster de local para a maioria do nó com a configuração de testemunha. O recurso de disco chamado *2 de disco de Cluster* está configurado como uma testemunha de disco.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

O exemplo a seguir altera a configuração de quorum no cluster de local para a maioria do nó com a configuração de testemunha. O recurso de compartilhamento de arquivo chamado *\\\CONTOSO-FS\\fsw* for configurado como uma testemunha de compartilhamento de arquivos.

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

O exemplo a seguir remove o voto quorum do nó *ContosoFCNode1* no cluster de local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

O exemplo a seguir adiciona o voto quorum ao nó *ContosoFCNode1* no cluster de local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

O exemplo a seguir permite que a propriedade **DynamicQuorum** do cluster *FC1 CONTOSO* (se ele foi desabilitado anteriormente):

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## Recuperar um cluster iniciando sem quorum

Um cluster que não tenha votos suficientes quorum não será iniciado. Como uma primeira etapa, você sempre deve confirmar a configuração de quorum de cluster e investigar por que o cluster não tem quorum. Isso pode acontecer se você tiver nós que pararam de responder, ou se o local principal não está acessível em um cluster multissite. Depois de identificar a causa da falha de cluster, você pode usar as etapas de recuperação descritas nesta seção.

>[!NOTE]
> * Se o serviço de Cluster parar porque quorum é perdido, 1177 de ID de evento aparece no log do sistema.
> * Sempre é necessário investigar por que o quorum do cluster foi perdido.
> * É sempre preferível a trazer uma testemunha de quorum ou de nó para um estado íntegro (ingresso no cluster) em vez de a partir do cluster sem quorum.

### Forçar iniciar nós de cluster

Depois de determinar que não é possível recuperar seu cluster ao trazer os nós ou testemunha de quorum para um estado íntegro, forçar o início do cluster se torna necessário. Forçar o início do cluster substitui as definições de configuração de quorum de cluster e inicia o cluster no modo de **ForceQuorum** .

Forçar um início quando ele não tem quorum do cluster pode ser especialmente útil em um cluster multissite. Considere um cenário de recuperação de desastres com um cluster que contenha sites separadamente localizados primários e de backup, *site* e *site b*. Se houver um desastre original no *site*, ele pode levar uma quantidade significativa de tempo para o site online novamente. Provavelmente, você desejaria forçar o *site b* ficar online, mesmo que ele não tem quorum.

Quando um cluster é iniciado no modo de **ForceQuorum** e depois que ele entrar novamente em votos de quorum suficiente, o cluster automaticamente deixa o estado forçado e ele se comporta normalmente. Portanto, não é necessário iniciar o cluster novamente normalmente. Se um nó perde o cluster e ele perde quorum, ele fica offline novamente porque ele não está mais no estado forçado. Para ativá-la novamente quando ele não tem quorum requer forçar o início sem quorum do cluster.

>[!IMPORTANT]
>* Depois que um cluster é iniciada de força, o administrador tem controle total do cluster.
>* O cluster usa a configuração de cluster no nó onde o cluster é força iniciada e duplica para todos os outros nós que estão disponíveis.
>* Se você forçar o início sem quorum do cluster, todas as definições de configuração de quorum são ignoradas enquanto o cluster permanece no modo de **ForceQuorum** . Isso inclui atribuições de voto nó específico e as configurações de gerenciamento de quorum dinâmico.

### Impedir que o quorum em nós de cluster restantes

Depois que tiver força iniciado do cluster em um nó, é necessário iniciar todos os nós restantes no cluster com uma configuração para impedir que o quorum. Um nó iniciado com uma configuração que impede o quorum indica ao serviço de Cluster para ingressar em um cluster existente em execução em vez de formando uma nova instância de cluster. Isso impede que os nós restantes formando um cluster de divisão que contém duas instâncias concorrentes.

Isso se torna necessário quando você precisa recuperar seu cluster em alguns cenários de recuperação de desastres multissite depois que tiver força iniciado no cluster no seu site, *b*de backup. Para adicionar a força iniciado cluster no *site b*, nós no seu site primário, *site*, precisa ser iniciado com o quorum impedido.

>[!IMPORTANT]
>Depois que um cluster é iniciada em um nó de força, recomendamos que você comece sempre nós restantes com o quorum impedido.

Veja como recuperar o cluster com o Gerenciador de Cluster de Failover:

1. No Gerenciador de Cluster de Failover, selecione ou especifique o cluster que você deseja recuperar.
2. Com o cluster selecionado, em **ações**, selecione **Iniciar de Cluster de força**.

    Gerenciador de Cluster de failover força inicia o cluster em todos os nós que podem ser acessados. O cluster usa a configuração atual do cluster ao iniciar.

>[!NOTE]
>* Para forçar o cluster para iniciar em um nó específico que contém uma configuração de cluster que você deseja usar, você deve usar os cmdlets do Windows PowerShell ou ferramentas de linha de comando equivalentes conforme apresentado após esse procedimento. 
> * Se você usar o Gerenciador de Cluster de Failover para se conectar a um cluster que é iniciada de força, e você usar a ação de **Iniciar o serviço de Cluster** para iniciar um nó, o nó será iniciado automaticamente com a configuração que impede que o quorum.

#### Comandos equivalentes do Windows PowerShell (Start-Clusternode)

O exemplo a seguir mostra como usar o cmdlet **Start-ClusterNode** para forçar o início do cluster no nó *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

Como alternativa, você pode digitar o seguinte comando localmente no nó:

```PowerShell
Net Start ClusSvc /FQ
```

O exemplo a seguir mostra como usar o cmdlet **Start-ClusterNode** para iniciar o serviço de Cluster com o quorum impedido no nó *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

Como alternativa, você pode digitar o seguinte comando localmente no nó:

```PowerShell
Net Start ClusSvc /PQ
```

## Considerações de quorum para configurações de recuperação de desastres

Esta seção resume as características e configurações de quorum para duas configurações de cluster multissite em implantações de recuperação de desastres. As diretrizes de configuração de quorum diferem dependendo se você precisar o failover automático ou failover manual para cargas de trabalho entre os sites. A configuração geralmente depende os contratos de nível de serviço (SLAs) que estão em vigor em sua organização para fornecer e dar suporte a cargas de trabalho em cluster em caso de falha ou desastre em um site.

### Failover automático

Nessa configuração, o cluster consiste em dois ou mais sites que podem hospedar funções de cluster. Se ocorrer uma falha em qualquer site, as funções de cluster devem failover automaticamente à lista de sites restantes. Portanto, o quórum do cluster deve ser configurado para que qualquer site pode sustentar uma falha de site concluída.

A tabela a seguir resume as considerações e recomendações para essa configuração.

<table>
<thead>
<tr class="header">
<th>Item</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Número de votos de nó por site</td>
<td>Deve ser igual</td>
</tr>
<tr class="even">
<td>Atribuição de nó de voto</td>
<td>Votos de nó não devem ser removidos porque todos os nós são igualmente importantes</td>
</tr>
<tr class="odd">
<td>Gerenciamento de quorum dinâmico</td>
<td>Deve ser habilitada</td>
</tr>
<tr class="even">
<td>Configuração de testemunha</td>
<td>Testemunha de compartilhamento é recomendável, configurados em um site que é separado de sites do cluster</td>
</tr>
<tr class="odd">
<td>Cargas de trabalho</td>
<td>Cargas de trabalho podem ser configuradas em qualquer um dos sites</td>
</tr>
</tbody>
</table>

#### Considerações adicionais para o failover automático

- Configurar a testemunha de compartilhamento de arquivo em um site separado é necessário para dar uma oportunidade de igual para sobreviver a cada site. Para obter mais informações, consulte [configuração de testemunha](#witness-configuration) anteriormente neste tópico.

### Failover manual

Nessa configuração, o cluster consiste em um site primário, *site*e um site de backup (recuperação), *b*. Funções de cluster são hospedadas no *site*. Devido à configuração de quorum do cluster, caso ocorra uma falha em todos os nós no *site*, o cluster parará de funcionar. Nesse cenário o administrador deve manualmente os serviços de cluster para o *site b* o failover e realizar etapas adicionais para recuperar o cluster.

A tabela a seguir resume as considerações e recomendações para essa configuração.

<table>
<thead>
<tr class="header">
<th>Item</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Número de votos de nó por site</td>
<td>Pode ser diferente</td>
</tr>
<tr class="even">
<td>Atribuição de nó de voto</td>
<td>- Nó votos não devem ser removidos em nós no local principal, <em>site</em><br />
- Nó votos deverá ser removido de nós no local de backup, <em>site b</em><br />
- Se ocorrer uma interrupção de longo prazo em <em>site</em>, votos devem ser atribuídos a nós em <em>site b</em> para habilitar a maioria de quorum nesse site como parte de recuperação</td>
</tr>
<tr class="odd">
<td>Gerenciamento de quorum dinâmico</td>
<td>Deve ser habilitada</td>
</tr>
<tr class="even">
<td>Configuração de testemunha</td>
<td>- Configure uma testemunha se houver um número par de nós em <em>site</em><br />
- Se for necessária uma testemunha, configure uma testemunha de compartilhamento de arquivo ou uma testemunha de disco que seja acessível apenas para nós no <em>site</em> (às vezes chamado de uma testemunha de disco assimétrico)</td>
</tr>
<tr class="odd">
<td>Cargas de trabalho</td>
<td>Usar proprietários preferenciais para manter as cargas de trabalho em execução em nós em <em>site</em></td>
</tr>
</tbody>
</table>

#### Considerações adicionais para o failover manual

- Inicialmente, somente os nós no *site* são configurados com votos de quorum. Isso é necessário para garantir que o estado de nós no *site b* não afeta o quórum do cluster.
- Etapas de recuperação podem variar dependendo se o *site* mantém uma falha temporária ou longo prazo.

## Mais informações

* [Clustering de failover](failover-clustering.md)
* [Cmdlets do PowerShell Windows de Clusters de failover](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Noções básicas sobre Cluster e Pool Quorum](../storage/storage-spaces/understand-quorum.md)