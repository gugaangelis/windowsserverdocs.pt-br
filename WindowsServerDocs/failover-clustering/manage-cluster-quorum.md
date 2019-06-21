---
title: Configurar e gerenciar o quorum em um cluster de failover
description: Informações detalhadas sobre como gerenciar o quorum do cluster em um cluster de failover do Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: bf854418e9efb7dbb5bd07ba86f29d84ba54d68a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280383"
---
# <a name="configure-and-manage-quorum"></a>Configurar e gerenciar o quorum

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece o fundamento e as etapas para configurar e gerenciar o quorum em um cluster de failover do Windows Server.

## <a name="understanding-quorum"></a>Quorum Noções básicas sobre

O quorum de um cluster é determinado pelo número de elementos votantes que devem fazer parte da associação de cluster ativo para que o cluster seja iniciado corretamente ou continue executando. Para obter uma explicação mais detalhada, consulte o [doc de quorum de cluster e pool de Noções básicas sobre](../storage/storage-spaces/understand-quorum.md).

## <a name="quorum-configuration-options"></a>Opções de configuração de quorum

O modelo de quorum no Windows Server é flexível. Se você precisar modificar a configuração de quorum para seu cluster, você pode usar o Assistente para configurar o Quorum de Cluster ou os cmdlets de Clusters de Failover do Windows PowerShell. Para ver as etapas e considerações sobre como configurar o quorum, consulte o [Configurar o quorum do cluster](#configure-the-cluster-quorum) posteriormente neste tópico.

A tabela a seguir lista as três opções de configuração de quorum disponíveis no Assistente para Configurar Quorum de Cluster.

| Opção  |Descrição  |
| --------- | ---------|
| Usar configurações típicas     |  O cluster atribui automaticamente um voto a cada nó e gerencia dinamicamente os votos do nó. Se for apropriado para seu cluster, e houver armazenamento compartilhado de cluster disponível, o cluster selecionará uma testemunha de disco. Essa opção é recomendada na maioria dos casos, pois o software de cluster escolhe automaticamente uma configuração de quorum e de testemunha que forneça a mais alta disponibilidade para seu cluster.       |
| Adicionar ou alterar a testemunha de quorum     |   Você pode adicionar, alterar ou remover um recurso de testemunha. Você pode configurar uma testemunha de disco ou compartilhamento de arquivos. O cluster atribui automaticamente um voto a cada nó e gerencia dinamicamente os votos do nó.      |
| Configuração avançada de quorum e seleção de testemunha     | Você deve selecionar essa opção somente quando tiver requisitos específicos ao aplicativo ou ao site para configurar o quorum. Você pode modificar a testemunha do quorum, adicionar ou remover votos de nó e escolher se o cluster deve gerenciar dinamicamente os votos do nó. Por padrão, os votos são atribuídos a todos os nós e os votos de nó são gerenciados dinamicamente.        |

Dependendo da opção de configuração de quorum escolhida e das configurações específicas, o cluster será configurado em um dos seguintes modos de quorum:

| Modo  | Descrição  |
| --------- | ---------|
| Maioria dos nós (sem testemunha)     |   Somente nós têm votos. Nenhuma testemunha de quorum é configurada. O quorum do cluster é a maioria dos nós votantes na associação de cluster ativa.      |
| Maioria de nós com testemunha (compartilhamento de arquivos ou disco)     |   Nós têm votos. Além disso, uma testemunha de quorum tem um voto. O quorum do cluster é a maioria dos nós votantes na associação de cluster ativa mais um voto de testemunha. Uma testemunha de quorum pode ser uma testemunha de disco designada ou uma testemunha de compartilhamento de arquivos designada. 
| Sem maioria (somente testemunha de disco)     | Nenhum nó tem votos. Somente uma testemunha de disco tem um voto. <br>O quorum do cluster é determinado pelo estado da testemunha de disco. Geralmente, esse modo não é recomendado, e ele não deve ser selecionado, já que cria um único ponto de falha para o cluster.       |

As subseções a seguir oferecem mais informações sobre definições de configuração de quorum avançada.

### <a name="witness-configuration"></a>Configuração de testemunha

Como regra geral, quando você configura um quorum, os elementos votantes no cluster devem ser um número ímpar. Portanto, se o cluster contém um número par de nós votantes, você deve configurar uma testemunha de disco ou uma testemunha de compartilhamento de arquivos. O cluster poderá sustentar um nó adicional inoperante. Além disso, adicionar um voto de testemunha habilita o cluster a continuar sendo executado caso metade dos nós de cluster fiquem inoperantes simultaneamente ou sejam desconectados.

Uma testemunha de disco geralmente será recomendada se todos os nós puderem ver o disco. Uma testemunha de compartilhamento de arquivos será recomendada quando você precisar considerar a recuperação de desastre multissite com armazenamento replicado. Será possível configurar uma testemunha de disco com armazenamento replicado somente se o fornecedor do armazenamento der suporte a acesso de leitura/gravação de todos os sites ao armazenamento replicado. <strong>*Uma testemunha de disco não é suportada com espaços de armazenamento diretos*</strong>.

A tabela a seguir fornece informações adicionais e considerações sobre os tipos de testemunha de quorum.

| Tipo de testemunha  | Descrição  | Requisitos e recomendações  |
| ---------    |---------        |---------                        |
| Testemunha de disco     |  <ul><li> LUN dedicado que armazena uma cópia do banco de dados do cluster</li><li> Mais útil para clusters com armazenamento compartilhado (não replicado)</li>       |  <ul><li>O tamanho do LUN deve ser, no mínimo, 512 MB</li><li> Deve ser dedicado a uso de cluster e não designado a uma função clusterizada</li><li> Deve ser incluído em um armazenamento clusterizado e passar nos testes de validação de armazenamento</li><li> Não pode ser um disco que seja um CSV (Volume Compartilhado Clusterizado)</li><li> Disco básico com um volume único</li><li> Não precisa ter uma letra da unidade</li><li> Pode ser formatado com NTFS ou ReFS</li><li> Também pode ser configurado com RAID de hardware para tolerância a falhas</li><li> Deve ser excluído dos backups e da verificação antivírus</li><li> Uma testemunha de disco não é suportada com espaços de armazenamento diretos</li>|
| Testemunha de compartilhamento de arquivos     | <ul><li>Compartilhamento de arquivos SMB configurado em um servidor de arquivos que executa Windows Server</li><li> Não armazena uma cópia do banco de dados do cluster</li><li> Mantém informações de cluster somente em um arquivo witness.log</li><li> Mais útil para clusters multissite com armazenamento replicado </li>       |  <ul><li>Deve ter um espaço livre mínimo de 5 MB</li><li> Deve ser dedicado a um único cluster e não usado para armazenar dados de usuário ou de aplicativo</li><li> Deve ter permissões de gravação habilitadas para objeto de computador do nome do cluster</li></ul><br>A seguir estão considerações adicionais para um servidor de arquivos que hospeda a testemunha de compartilhamento de arquivos:<ul><li>Um único servidor de arquivos pode ser configurado com testemunhas de compartilhamento de arquivos para vários clusters.</li><li> O servidor de arquivos deve estar em um site separado da carga de trabalho do cluster. Isso permite a mesma oportunidade para que qualquer site de cluster sobreviva se a comunicação da rede de site a site for perdida. Se o servidor de arquivos estiver no mesmo site, esse site se tornará o site primário, e ele será o único site que poderá atingir o compartilhamento de arquivos.</li><li> O servidor de arquivos pode ser executado em uma máquina virtual, se esta não estiver hospedada no mesmo cluster que utiliza a testemunha de compartilhamento de arquivos.</li><li> Para alta disponibilidade, o servidor de arquivos pode ser configurado em um cluster de failover separado. </li>      |
| Testemunha de nuvem     |  <ul><li>Um arquivo de testemunha armazenado no armazenamento de BLOBs do Azure</li><li> Recomendado quando todos os servidores no cluster possuem uma conexão de Internet confiável.</li>      |  Ver [implantar uma testemunha de nuvem](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).       |

### <a name="node-vote-assignment"></a>Atribuição de voto de nó

Como uma opção de configuração de quorum avançada, você pode escolher Atribuir ou remover votos de quorum em uma base por nó. Por padrão, todos os nós são votos atribuídos. Independentemente da atribuição de voto, todos os nós continuam a funcionar no cluster, receber atualizações do banco de dados do cluster e podem hospedar aplicativos.

Você pode desejar remover votos dos nós em determinadas configurações de recuperação de desastre. Por exemplo, em um cluster multissite, você pode remover votos dos nós em um site de backup de forma que aqueles nós não afetem os cálculos de quorum. Essa configuração é recomendada somente para failover manual nos sites. Para obter mais informações, consulte [Considerações de quorum para configurações de recuperação de desastre](#quorum-considerations-for-disaster-recovery-configurations) posteriormente neste tópico.

O voto configurado de um nó pode ser verificado procurando a **NodeWeight** propriedade comum do nó de cluster usando o [Get-ClusterNode](https://technet.microsoft.com/library/hh847268.aspx)cmdlet do Windows PowerShell. Um valor de 0 indica que o nó não tem um voto de quorum configurado. Um valor de 1 indica que o voto de quorum do nó está atribuído e é gerenciado pelo cluster. Para obter mais informações sobre o gerenciamento de votos de nó, consulte [Gerenciamento dinâmico de quorum](#dynamic-quorum-management) posteriormente nesse tópico.

A atribuição de voto para todos os nós de cluster pode ser verificada usando o teste de validação **Validar Quorum de Cluster** .

#### <a name="additional-considerations-for-node-vote-assignment"></a>Considerações adicionais para a atribuição de voto de nó

  - Na atribuição de voto de nó não é recomendado impor um número ímpar de nós votantes. Em vez disso, você deve configurar uma testemunha de disco ou testemunha de compartilhamento de arquivos. Para obter mais informações, consulte [configuração de testemunha](#witness-configuration) mais adiante neste tópico.
  - Se o gerenciamento de quorum dinâmico for habilitado, somente os nós configurados para ter votos de nós atribuídos poderão ter votos atribuídos ou removidos dinamicamente. Para obter mais informações, consulte [Gerenciamento dinâmico de quorum](#dynamic-quorum-management) posteriormente neste tópico.

### <a name="dynamic-quorum-management"></a>Gerenciamento dinâmico de quorum

No Windows Server 2012, como uma opção de configuração de quorum avançada, você pode escolher habilitar o gerenciamento de quorum dinâmico pelo cluster. Para obter mais detalhes sobre o quorum como dinâmico, consulte [essa explicação](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

Com o gerenciamento de quorum dinâmico, também é possível que um cluster seja executado no último nó de cluster sobrevivente. Pelo ajuste dinâmico do requisito de maioria de quorum, o cluster pode sustentar desligamentos de nó sequenciais para um único nó.

O voto dinâmico atribuído de cluster de um nó pode ser verificado com o **DynamicWeight** propriedade comum do nó de cluster usando o [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) cmdlet do Windows PowerShell. Um valor de 0 indica que o nó não tem um voto de quorum. Um valor de 1 indica que o nó tem um voto de quorum.

A atribuição de voto para todos os nós de cluster pode ser verificada usando o teste de validação **Validar Quorum de Cluster** .

#### <a name="additional-considerations-for-dynamic-quorum-management"></a>Considerações adicionais para gerenciamento de quorum dinâmico

- O gerenciamento de quorum dinâmico não permite que o cluster sustente uma falha simultânea de uma maioria de membros votantes. Para continuar executando, o cluster deve sempre ter uma maioria de quorum no momento de desligamento ou falha do nó.

- Se você removeu explicitamente o voto de um nó, o cluster não poderá adicionar ou remover dinamicamente esse voto.
- Quando espaços de armazenamento diretos está habilitado, o cluster só pode suportar duas falhas de nó. Isso será explicado mais o [seção de quorum do pool](../storage/storage-spaces/understand-quorum.md)

## <a name="general-recommendations-for-quorum-configuration"></a>Recomendações gerais para configuração de quorum

O software do cluster configura automaticamente o quorum para um novo cluster, de acordo com o número de nós configurado e a disponibilidade de armazenamento compartilhado. Essa geralmente é a configuração de quorum mais apropriada para esse cluster. Entretanto, é uma boa ideia examinar a configuração de quorum depois de criar o cluster, e antes de colocá-lo em produção. Para exibir a configuração de quorum do cluster detalhada, você pode usar o Assistente de configuração, validar ou o [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) cmdlet do Windows PowerShell, para executar o **validar configuração de Quorum** de teste. No Gerenciador de Cluster de Failover, a configuração básica do quorum é exibida nas informações de resumo para o cluster selecionado, ou você pode examinar as informações sobre recursos de quorum que retorna quando você executa o [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) Cmdlet do Windows PowerShell.

A qualquer momento, você pode executar o teste **Validar Configuração de Quorum** para validar que a configuração de quorum é ideal para seu cluster. A saída do teste indica se uma alteração na configuração de quorum é recomendada e as configurações que são ideais. Se uma alteração for recomendada, você poderá usar o Assistente para Configurar Quorum do Cluster para aplicar as configurações recomendadas.

Depois que o cluster estiver em produção, não altere a configuração de quorum, a menos que você tenha determinado que a alteração é apropriada para seu cluster. Você pode desejar considerar a alteração da configuração de quorum nas seguintes situações:

- Adicionar ou remover nós
- Adicionar ou remover armazenamento
- Uma falha de longo prazo em nó ou em testemunha
- Recuperar um cluster em um cenário de recuperação de desastre multissite

Para obter mais informações sobre como validar um cluster de failover, consulte [Validar hardware para um cluster de failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="configure-the-cluster-quorum"></a>Configurar o quorum do cluster

Você pode configurar as definições de quorum do cluster usando o Gerenciador de Cluster de Failover ou os cmdlets de Clusters de Failover do Windows PowerShell.

> [!IMPORTANT]
> Geralmente é melhor usar a configuração de quorum recomendada pelo Assistente para Configurar Quorum do Cluster. Recomendamos a personalização da configuração de quorum somente se você tiver determinado que a alteração é apropriada para seu cluster. Para obter mais informações, consulte [Recomendações gerais para configuração de quorum](#general-recommendations-for-quorum-configuration) neste tópico.

### <a name="configure-the-cluster-quorum-settings"></a>Configurar as definições de quorum do cluster

A associação no grupo local **Administradores** em cada servidor com cluster, ou equivalente, é a permissão mínima necessária para concluir esse procedimento. Também, a conta que você usa deve ser uma conta de usuário do domínio.

> [!NOTE]
> Você pode alterar a configuração de quorum do cluster sem parar o cluster ou tornar offline os recursos dele.

### <a name="change-the-quorum-configuration-in-a-failover-cluster-by-using-failover-cluster-manager"></a>Alterar a configuração de quorum em um cluster de failover usando o Gerenciador de Cluster de Failover

1. No Gerenciador de Cluster de Failover, selecione ou especifique o cluster que deseja alterar.
2. Com o cluster selecionado, em **ações**, selecione **mais ações**e, em seguida, selecione **configurar quorum do Cluster**. O Assistente para Configurar Quorum do Cluster aparece. Selecione **Avançar**.
3. Na página **Selecionar Opção de Configuração de Quorum** , selecione uma das três opções de configuração e conclua as etapas para essa opção. Antes de configurar o quorum, você pode examinar suas escolhas. Para obter mais informações sobre as opções, consulte [quorum Noções básicas sobre](#understanding-quorum), anteriormente neste tópico.

    - Para permitir que o cluster redefina automaticamente as configurações do quorum ideais para sua configuração de cluster atual, selecione **usar configurações típicas** e, em seguida, conclua o assistente.
    - Para adicionar ou alterar a testemunha de quorum, selecione **adicionar ou alterar a testemunha de quorum**e, em seguida, conclua as etapas a seguir. Para obter informações e considerações sobre configurar uma testemunha de quorum, consulte [Configuração de testemunha](#witness-configuration) anteriormente neste tópico.

      1. Na página **Selecionar Testemunha de Quorum** , selecione uma opção para configurar uma testemunha de disco ou uma testemunha de compartilhamento de arquivos. O assistente indica as opções de seleção de testemunha recomendadas para seu cluster.

          > [!NOTE]
          > Você pode selecionar **Não configurar uma testemunha de quorum** e concluir o assistente. Se você tiver um número par de nós votantes em seu cluster, isso pode não ser uma configuração recomendada.

      2. Se você selecionar a opção para configurar uma testemunha de disco, na página **Configurar Testemunha de Armazenamento** , selecione o volume de armazenamento que deseja atribuir como a testemunha de disco e conclua o assistente.
      3. Se você selecionar a opção para configurar uma testemunha de compartilhamento de arquivos na página **Configurar Testemunha de Compartilhamento de Arquivo**, digite ou procure um compartilhamento de arquivos que será usado como o recurso de testemunha e conclua o assistente.

    - Para configurar o gerenciamento de quorum e para adicionar ou alterar a testemunha de quorum, selecione **seleção de configuração e a testemunha de quorum avançada**e, em seguida, conclua as etapas a seguir. Para obter informações e considerações sobre as configurações de quorum avançadas, consulte [Atribuição de voto de nó](#node-vote-assignment) e [Gerenciamento dinâmico de quorum](#dynamic-quorum-management) anteriormente neste tópico.

      1. Na página **Selecionar Configuração de Votação** , selecione uma opção para atribuir votos aos nós. Por padrão, todos os nós são atribuídos a um voto. Entretanto, para determinados cenários, você pode atribuir votos somente a um subconjunto de nós.

          > [!NOTE]
          > Você também pode selecionar **Nenhum nó**. Isso geralmente não é recomendado, porque não permite que os nós participem na votação de quorum, e requer a configuração de uma testemunha de disco. A testemunha de disco se torna o ponto único de falha para o cluster.

      2. Na página **Configurar Gerenciamento de Quorum**, você pode habilitar ou desabilitar a opção **Permitir que o cluster gerencie dinamicamente a atribuição de votos de nós**. Selecionar essa opção geralmente aumenta a disponibilidade do cluster. Por padrão, a opção é habilitada e é altamente recomendado não desabilitar essa opção. Essa opção permite ao cluster continuar a ser executado em cenários de falha que não são possíveis quando essa opção está desabilitada.
      3. Na página **Selecionar Testemunha de Quorum** , selecione uma opção para configurar uma testemunha de disco ou uma testemunha de compartilhamento de arquivos. O assistente indica as opções de seleção de testemunha recomendadas para seu cluster.

          > [!NOTE]
          > Você pode selecionar **Não configurar uma testemunha de quorum**e concluir o assistente. Se você tiver um número par de nós votantes em seu cluster, isso pode não ser uma configuração recomendada.

      4. Se você selecionar a opção para configurar uma testemunha de disco, na página **Configurar Testemunha de Armazenamento** , selecione o volume de armazenamento que deseja atribuir como a testemunha de disco e conclua o assistente.
      5. Se você selecionar a opção para configurar uma testemunha de compartilhamento de arquivos na página **Configurar Testemunha de Compartilhamento de Arquivo**, digite ou procure um compartilhamento de arquivos que será usado como o recurso de testemunha e conclua o assistente.

4. Selecione **Avançar**. Confirme suas seleções na página de confirmação que aparece e, em seguida, selecione **próxima**.

Depois que o assistente é executado e o **resumo** página será exibida se você quiser exibir um relatório das tarefas executadas pelo assistente, selecione **Exibir relatório**. O relatório mais recente permanecerá na <em>systemroot</em> **\\Cluster\\relatórios** pasta com o nome **Quorumconfiguration**.

> [!NOTE]
> Depois de configurar o quorum do cluster, recomendamos que você execute o teste **Validar Configuração de Quorum** para verificar as configurações de quorum atualizadas.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes do Windows PowerShell

Os exemplos a seguir mostram como usar o [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) cmdlet e outros cmdlets do Windows PowerShell para configurar o quorum do cluster.

O exemplo a seguir altera a configuração de quorum no cluster *CONTOSO-FC1* para uma configuração simples de maioria dos nós sem testemunha de quorum.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

O exemplo a seguir altera a configuração de quorum no cluster local para uma maioria dos nós com configuração de testemunha. O recurso de disco denominado *Disco de Cluster 2* é configurado como uma testemunha de disco.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

O exemplo a seguir altera a configuração de quorum no cluster local para uma maioria dos nós com configuração de testemunha. O recurso de compartilhamento de arquivos denominado  *\\ \\FS CONTOSO\\fsw* está configurado como uma testemunha de compartilhamento de arquivos.

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

O exemplo a seguir remove o voto de quorum do nó *ContosoFCNode1* no cluster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

O exemplo a seguir adiciona o voto de quorum ao nó *ContosoFCNode1* no cluster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

O exemplo a seguir habilita a propriedade **DynamicQuorum** do cluster *CONTOSO-FC1* (se ele foi previamente desabilitado):

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## <a name="recover-a-cluster-by-starting-without-quorum"></a>Recuperar um cluster iniciando sem quorum

Um cluster que não tem votos de quorum suficientes não será iniciado. Como uma primeira etapa, você sempre deve confirmar a configuração de quorum do cluster e investigar por que o cluster não tem mais quorum. Isso poderá acontecer se você tiver nós que pararam de responder, ou se o site primário não for acessível em um cluster multissite. Após identificar a causa raiz para a falha do cluster, você poderá usar as etapas de recuperação descritas nesta seção.

> [!NOTE]
> * Se o Serviço de cluster é interrompido devido a uma perda de quorum, aparecerá a ID de Evento 1177 no log do sistema.
> * Isso sempre será necessário para investigar por que o quorum do cluster foi perdido.
> * Sempre é preferível fazer com que uma testemunha de nó ou quorum fique no estado íntegro (ingressar no cluster), em vez de iniciar o cluster sem quorum.

### <a name="force-start-cluster-nodes"></a>Forçar a inicialização de nós de cluster

Depois de determinar que não é possível recuperar seu cluster fazendo com que a testemunha de nós ou de quorum fique em um estado íntegro, será necessário forçar a inicialização do cluster. Forçar a inicialização do cluster substitui as configurações de quorum de seu cluster e inicia o cluster no modo **ForceQuorum**.

Forçar a inicialização de um cluster quando ele não tem quorum pode ser especialmente útil em um cluster multissite. Considere um cenário de recuperação de desastre com um cluster que contém sites primários e de backup, *SiteA* e *SiteB*. Se houver um desastre genuíno no *SiteA*, levará um período tempo significativo até que o site volte a ficar online. Você provavelmente iria querer forçar o *SiteB* a ficar online, mesmo se ele não tivesse quorum.

Quando um cluster é iniciado no modo **ForceQuorum**, e depois de recobrar votos de quorum suficientes, ele automaticamente deixa o estado forçado e se comporta normalmente. Assim, não é necessário reiniciar o cluster normalmente. Se o cluster perder um nó e este perder o quorum, ele ficará offline novamente, porque não estará mais no estado forçado. Para colocá-lo online novamente quando ele não tem quorum, é necessário forçar o cluster a iniciar sem quorum.

> [!IMPORTANT]
> * Depois que um cluster é forçado a iniciar, o administrador tem total controle sobre ele.
> * O cluster usa a configuração do cluster no nó em que é forçado a iniciar, e replica essa configuração a todos os outros nós disponíveis.
> * Se você forçar o cluster a iniciar sem quorum, todas as configurações do quorum serão ignoradas enquanto o cluster permanecer no modo **ForceQuorum**. Isso inclui atribuições de voto de nó específicas e configurações de gerenciamento de quorum dinâmicas.

### <a name="prevent-quorum-on-remaining-cluster-nodes"></a>Impedir quorum nos nós de cluster restantes

Depois de ter forçado a inicialização do cluster em um nó, será necessário iniciar os nós restantes em seu cluster com uma configuração para impedir quorum. Um nó iniciado com uma configuração que impede quorum indica para o Serviço de cluster ingressar em um cluster em execução existente, em vez de formar uma nova instância de cluster. Isso impede que os nós restantes formem um cluster dividido que possa conter duas instâncias concorrentes.

Isso é necessário quando você precisa recuperar seu cluster em alguns cenários de recuperação de desastre multissite, depois de ter forçado a inicialização do cluster em seu site de backup, o *SiteB*. Para ingressar o cluster iniciado à força no *SiteB*, os nós em seu site primário, o *SiteA*, precisarão ser iniciados com o quorum impedido.

> [!IMPORTANT]
> Depois que um cluster é iniciado à força em um nó, recomendamos que você sempre inicie os nós restantes com o quorum impedido.

Aqui está como recuperar o cluster com o Gerenciador de Cluster de Failover:

1. No Gerenciador de Cluster de Failover, selecione ou especifique o cluster que você deseja recuperar.
2. Com o cluster selecionado, em **ações**, selecione **forçar início do Cluster**.

    O Gerenciador de Cluster de Failover força a inicialização do cluster em todos os nós acessíveis. O cluster usa a configuração de cluster atual ao iniciar.

> [!NOTE]
> * Para forçar o cluster a iniciar em um nó específico que contém uma configuração de cluster que você deseja usar, você deve usar os cmdlets do Windows PowerShell ou as ferramentas de linha de comando equivalentes, conforme apresentado após esse procedimento. 
> * Se usar o Gerenciador de Cluster de Failover para se conectar a um cluster com inicialização forçada, e usar a ação **Iniciar Serviço de Cluster** para iniciar um nó, este será automaticamente iniciado com a configuração que impede quorum.

#### <a name="windows-powershell-equivalent-commands-start-clusternode"></a>Comandos equivalentes do Windows PowerShell (Start-Clusternode)

O exemplo a seguir mostra como usar o cmdlet **Start-ClusterNode** para forçar a inicialização do cluster no nó *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

Você também pode digitar o seguinte comando localmente no nó:

```PowerShell
Net Start ClusSvc /FQ
```

O exemplo a seguir mostra como usar o cmdlet **Start-ClusterNode** para iniciar o Serviço de cluster com o quorum impedido no nó *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

Você também pode digitar o seguinte comando localmente no nó:

```PowerShell
Net Start ClusSvc /PQ
```

## <a name="quorum-considerations-for-disaster-recovery-configurations"></a>Considerações de quorum para configurações de recuperação de desastre

Esta seção resume as características e configurações de quorum para duas configurações de cluster multissite em implantações de recuperação de desastre. As diretrizes de configuração de quorum diferem dependendo de sua necessidade de failover automático ou failover manual para cargas de trabalho entre sites. Sua configuração geralmente depende dos SLAs (contratos de nível de serviço) que estabelecidos em sua organização para fornecer cargas de trabalho clusterizadas, e dar suporte a elas, caso ocorra uma falha ou um desastre em um site.

### <a name="automatic-failover"></a>Failover automático

Nessa configuração, o cluster consiste em um ou mais sites que podem hospedar funções clusterizadas. Se ocorrer uma falha em qualquer site, espera-se que as funções clusterizadas façam failover automático nos sites restantes. Portanto, o quorum do cluster deve ser configurado de forma que qualquer site possa sustentar uma falha completa do site.

A tabela a seguir resume considerações e recomendações para essa configuração.

| Item  | Descrição  |
| ---------| ---------|
| Número de votos de nó por site     | Deve ser igual       |
| Atribuição de voto de nó     |  Votos de nó não devem ser removidos porque todos os nós são igualmente importantes       |
| Gerenciamento dinâmico de quorum     |   Deve ser habilitado      |
| Configuração de testemunha     |  A testemunha de compartilhamento de arquivos é recomendada, configurada em um site separado dos sites do cluster       |
| Cargas de trabalho     |  As cargas de trabalho podem ser configuradas em qualquer um dos sites       |

#### <a name="additional-considerations-for-automatic-failover"></a>Considerações adicionais para o failover automático

- É necessário configurar a testemunha de compartilhamento de arquivos em um site separado, a fim de que cada site tenha a mesma oportunidade de sobreviver. Para obter mais informações, consulte [Configuração de testemunha](#witness-configuration) anteriormente neste tópico.

### <a name="manual-failover"></a>Failover manual

Nessa configuração, o cluster consiste em um site primário, o *SiteA*, e um site de backup (recuperação), o *SiteB*. Funções clusterizadas são hospedadas no *SiteA*. Por causa da configuração de quorum do cluster, se ocorrer uma falha em todos os nós no *SiteA*, o cluster parará de funcionar. Nesse cenário, o administrador deve fazer failover manual nos serviços do cluster para o *SiteB* , e executar etapas adicionais para recuperar o cluster.

A tabela a seguir resume considerações e recomendações para essa configuração.

| Item   |Descrição  |
| ---------| ---------|
| Número de votos de nó por site     |  <ul><li> Votos de nó não devem ser removidos dos nós no site primário, o **SiteA**</li><li>Votos de nó devem ser removidos dos nós no site de backup, o **SiteB**</li><li>Se ocorrer uma interrupção de longo prazo no **SiteA**, os votos deverão ser atribuídos aos nós no **SiteB**, a fim de habilitar uma maioria de quorum no site como parte da recuperação</li>       |
| Gerenciamento dinâmico de quorum     |  Deve ser habilitado       |
| Configuração de testemunha     |  <ul><li>Configure uma testemunha se houver um número par de nós no **SiteA**</li><li>Se for necessária uma testemunha, configure uma testemunha de compartilhamento de arquivos ou uma testemunha de disco que seja acessível somente aos nós no **SiteA** (às vezes chamada de testemunha de disco assimétrico)</li>       |
| Cargas de trabalho     |  Use proprietários preferenciais para manter cargas de trabalho em execução nos nós no **SiteA**       |

#### <a name="additional-considerations-for-manual-failover"></a>Considerações adicionais para failover manual

- Somente os nós no *SiteA* são inicialmente configurados com votos de quorum. Isso é necessário para assegurar que o estado dos nós no *SiteB* não afete o quorum do cluster.
- As etapas de recuperação podem variar, dependendo se o *SiteA* sustenta uma falha temporária ou uma falha de longo prazo.

## <a name="more-information"></a>Mais informações

* [Clustering de failover](failover-clustering.md)
* [Cmdlets de Clusters de failover do Windows PowerShell](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Noções básicas sobre Cluster e o Quorum do Pool](../storage/storage-spaces/understand-quorum.md)