---
title: Criar um cluster de failover
description: Como criar um cluster de failover para o Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f919e69488c4f2272ddd07e535ba4e2248ddf79c
ms.sourcegitcommit: 5cbaca9685720f11d896c4ca167c86e74c032feb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6068974"
---
# Criar um cluster de failover

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Este tópico mostra como criar um cluster de failover usando o snap-in Gerenciador de Cluster de Failover ou o Windows PowerShell. Esse tópico abrange uma implantação típica, onde os objetos de computador para o cluster e suas funções de cluster associadas são criados no Active Directory Domain Services (AD DS). Se você estiver implantando um cluster de espaços de armazenamento diretos, em vez disso, consulte [Implantar espaços de armazenamento diretos](../storage/storage-spaces/deploy-storage-spaces-direct.md).

Você também pode implantar um cluster desanexado do Active Directory. Esse método de implantação permite que você crie um cluster de failover sem permissões para criar objetos de computador no AD DS ou a necessidade de solicitar nesse computador objetos inseridos no AD DS. Essa opção só está disponível por meio do Windows PowerShell e só é recomendada para cenários específicos. Para obter mais informações, consulte [implantar um Cluster Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### Lista de verificação: Criar um cluster de failover

|Status|Tarefa|Referência|
|:---:|---|---|
|☐|Verifique se os pré-requisitos|[Verifique se os pré-requisitos](#verify-the-prerequisites)|
|☐|Instalar o recurso de Clustering de Failover em cada servidor que você deseja adicionar como um nó de cluster|[Instalar o recurso de Clustering de Failover](#install-the-failover-clustering-feature)|
|☐|Execute o Assistente de validação de Cluster para validar a configuração|[Validar a configuração](#validate-the-configuration)|
|☐|Execute o Assistente de criação de Cluster para criar o cluster de failover|[Criar o cluster de failover](#create-the-failover-cluster)|
|☐|Criar funções de cluster para cargas de trabalho de cluster de host|[Criar funções de cluster](#create-clustered-roles)|

## Verifique se os pré-requisitos

Antes de começar, verifique se os pré-requisitos a seguir:

- Certifique-se de que todos os servidores que você deseja adicionar como nós de cluster estão executando a mesma versão do Windows Server.
- Examine os requisitos de hardware para certificar-se de que sua configuração é suportada. Para obter mais informações, consulte os [requisitos de Hardware de Clustering de Failover e opções de armazenamento](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Se você estiver criando um cluster de espaços de armazenamento diretos, consulte [os requisitos de hardware espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Para adicionar o armazenamento de cluster durante a criação do cluster, certifique-se de que todos os servidores podem acessar o armazenamento. (Você também pode adicionar armazenamento clusterizado depois de criar o cluster.)
- Certifique-se de que todos os servidores que você deseja adicionar como nós de cluster estão ingressados no mesmo domínio do Active Directory.
- (Opcional) Criar uma unidade organizacional (UO) e mover as contas de computador para os servidores que você deseja adicionar como nós de cluster para a unidade Organizacional. Como prática recomendada, é recomendável que você coloque clusters de failover em sua própria OU no AD DS. Isso pode ajudá-lo a controlar melhor quais configurações de política de grupo ou as configurações de modelo de segurança afetam os nós do cluster. Isolando clusters em sua própria unidade Organizacional, ele também ajuda a evitar contra exclusão acidental de objetos de computador do cluster.

Além disso, verifique se os seguintes requisitos de conta:

- Certifique-se de que a conta que você deseja usar para criar o cluster é um usuário de domínio que tenha direitos de administrador em todos os servidores que você deseja adicionar como nós de cluster.
- Certifique-se de que qualquer um dos seguintes é verdadeiro:
    - O usuário que cria o cluster tem a permissão de **computador criar objetos** na UO ou o contêiner onde residem os servidores que serão formar o cluster.
    - Se o usuário não tiver a permissão de **objetos de computador criar** , peça ao administrador de domínio para inserir um objeto de computador de cluster para cluster. Para obter mais informações, consulte [Prestage Cluster Computer Objects in Active Directory Domain Services](prestage-cluster-adds.md).

>[!NOTE]
>Esse requisito não se aplica se você quiser criar um cluster desanexado do Active Directory no Windows Server 2012 R2. Para obter mais informações, consulte [implantar um Cluster Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## Instalar o recurso de Clustering de Failover

Você deve instalar o recurso de Clustering de Failover em cada servidor que você deseja adicionar como um nó de cluster de failover.

### Instalar o recurso de Clustering de Failover

1. Inicie o Gerenciador do servidor.
2. No menu **Gerenciar** , selecione **Adicionar funções e recursos**.
3. Na página **antes de começar** , selecione **Avançar**.
4. Na página **Selecionar o tipo de instalação** , selecione **instalação baseado em função ou recurso**e selecione **Avançar**.
5. Na página **Selecionar servidor de destino** , selecione o servidor em que você deseja instalar o recurso e, em seguida, selecione **Avançar**.
6. Na página **Selecionar funções de servidor** , selecione **Avançar**.
7. Na página **Selecionar recursos** , marque a caixa de seleção **Clustering de Failover** .
8. Para instalar as ferramentas de gerenciamento de cluster de failover, selecione **Adicionar recursos**e, em seguida, selecione **Avançar**.
9. Na página **Confirmar seleções de instalação** , selecione a **instalação**.
<br>Uma reinicialização do servidor não é necessária para o recurso de cluster de Failover.

10. Quando a instalação for concluída, selecione **Fechar**.
11. Repita esse procedimento em cada servidor que você deseja adicionar como um nó de cluster de failover.

>[!NOTE]
>Depois de instalar o recurso de cluster de Failover, é recomendável que você aplique as atualizações mais recentes do Windows Update. Além disso, para um cluster de failover baseado no Windows Server 2012, leia o artigo do Microsoft Support [clusters de hotfixes recomendados e atualizações para o failover baseado no Windows Server 2012](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) e instalar as atualizações que se aplicam.

## Validar a configuração

Antes de criar o cluster de failover, é altamente recomendável que você valide a configuração para certificar-se de que o hardware e as configurações de hardware são compatíveis com o cluster de failover. A Microsoft oferece suporte a uma solução de cluster somente se a configuração completa passar na validação de todos os testes e se todo o hardware é certificado para a versão do Windows Server que executam os nós do cluster.

>[!NOTE]
>Você deve ter pelo menos dois nós para executar todos os testes. Se você tiver apenas um nó, muitos dos testes críticas de armazenamento não são executados.

### Executar testes de validação de cluster

1. Em um computador que possui as ferramentas de gerenciamento de Cluster de Failover instalado a partir as ferramentas de administração de servidor remoto ou em um servidor em que você instalou o recurso de cluster de Failover, inicie o Gerenciador de Cluster de Failover. Para fazer isso em um servidor, inicie o Gerenciador do servidor e, em seguida, no menu **Ferramentas** , selecione **O Gerenciador de Cluster de Failover**.
2. No painel do **Gerenciador de Cluster de Failover** , em **gerenciamento**, selecione **Validar configuração**.
3. Na página **Antes de começar** , selecione **Avançar**.
4. Na página **Selecione servidores ou em um Cluster** , na caixa **Digite o nome** , insira o nome de NetBIOS ou o nome de domínio totalmente qualificado de um servidor que você planeja adicionar como um nó de cluster de failover e, em seguida, selecione **Add**. Repita esta etapa para cada servidor que você deseja adicionar. Para adicionar vários servidores ao mesmo tempo, separe os nomes por uma vírgula ou por um ponto e vírgula. Por exemplo, insira os nomes no formato *Server1. contoso.com, server2.contoso.com*. Quando tiver terminado, selecione **Avançar**.
5. Na página **Opções de teste** , selecione **Executar todos os testes (recomendados)** e, em seguida, selecione **Avançar**.
6. Na página de **confirmação** , selecione **Avançar**.

    A página Validando exibe o status dos testes em execução.
7. Na página de **Resumo** , siga um destes procedimentos:
    
      - Se os resultados indicam que os testes concluídos com êxito e a configuração é adequada para clustering, e você deseja criar o cluster imediatamente, certifique-se de que a caixa de seleção de **criar o cluster agora usando os nós validados** é selecionada e, em seguida, Selecione **Concluir**. Em seguida, continue para a etapa 4 do procedimento [criar o cluster de failover](#create-the-failover-cluster) .
      - Se os resultados indicam que houve falhas ou avisos, selecione **Exibir o relatório** para exibir os detalhes e determinar quais problemas devem ser corrigidos. Perceba que um aviso para um teste de validação específica indica que esse aspecto do cluster de failover pode ter suporte, mas pode não atender às práticas recomendadas.
        
        >[!NOTE]
        >Se você receber um aviso para o teste de validar armazenamento espaços persistente reserva, consulte o postagem do blog [Aviso de validação de Cluster de Failover do Windows indica que os discos não dão suporte as reservas persistentes para espaços de armazenamento](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) para obter mais informações.

Para obter mais informações sobre testes de validação de hardware, consulte [Validar o Hardware para um Cluster de Failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## Criar o cluster de failover

Para concluir essa etapa, certifique-se de que a conta de usuário que você faça logon como atende aos requisitos descritos na seção [Verificar os pré-requisitos](#verify-the-prerequisites) deste tópico.

1. Inicie o Gerenciador do servidor.
2. No menu **Ferramentas** , selecione o **Gerenciador de Cluster de Failover**.
3. No painel do **Gerenciador de Cluster de Failover** , em **gerenciamento**, selecione **Criar o Cluster**.
    
    Abre o Assistente de criação de Cluster.
4. Na página **Antes de começar** , selecione **Avançar**.
5. Se a página **Selecionar servidores** aparecer, na caixa **Digite o nome** , insira o nome de NetBIOS ou o nome de domínio totalmente qualificado de um servidor que você planeja adicionar como um nó de cluster de failover e, em seguida, selecione **Add**. Repita esta etapa para cada servidor que você deseja adicionar. Para adicionar vários servidores ao mesmo tempo, separe os nomes por uma vírgula ou um ponto e vírgula. Por exemplo, insira os nomes no formato *Server1. contoso.com; server2.contoso.com*. Quando tiver terminado, selecione **Avançar**.
    
    >[!NOTE]
    >Se você optar por criar o cluster imediatamente após a execução de validação no [procedimento de validação de configuração](#validate-the-configuration), você não verá a página **Selecionar servidores** . Os nós que foram validados são adicionados automaticamente para o Assistente de criação de Cluster para que você não precise inseri-las novamente.
6. Se você ignorou validação anteriormente, a página de **Aviso de validação** será exibida. É altamente recomendável que você execute validação de cluster. Somente os clusters que passa todos os testes de validação são suportados pela Microsoft. Para executar os testes de validação, selecione **Sim**e, em seguida, selecione **Avançar**. Conclua o Assistente para configuração de validar conforme descrito em [Validar a configuração](#validate-the-configuration).
7. Na página do **Ponto de acesso para administrar o Cluster** , faça o seguinte:
    
    1. Na caixa de **Nome do Cluster** , insira o nome que você deseja usar para administrar o cluster. Antes de você, revise as seguintes informações:
        
          - Durante a criação do cluster, esse nome é registrado como o objeto de computador de cluster (também conhecido como o *objeto de nome do cluster* ou *CNO*) no AD DS. Se você especificar um nome de NetBIOS para o cluster, o CNO é criado no mesmo local em que residem os objetos de computador para os nós de cluster. Isso pode ser o recipiente computadores padrão ou uma UO.
          - Para especificar um local diferente para o CNO, você pode inserir o nome distinto de uma UO na caixa de **Nome do Cluster** . Por exemplo: *CN = ClusterName, UO = Clusters, DC = Contoso, DC = com*.
          - Se um administrador de domínio tenha inserido o CNO em uma UO diferente de onde residem os nós do cluster, especifique o nome diferenciado que fornece o administrador de domínio.
    2. Se o servidor não tiver um adaptador de rede que está configurado para usar o DHCP, você deve configurar um ou mais endereços IP estáticos para o cluster de failover. Marque a caixa de seleção ao lado de cada rede que você deseja usar para o gerenciamento de cluster. Selecione o campo de **endereço** ao lado de uma rede selecionada e, em seguida, insira o endereço IP que você deseja atribuir ao cluster. Esse endereço IP (ou endereços) será associados com o nome do cluster no sistema de nome de domínio (DNS).
    3. Quando tiver terminado, selecione **Avançar**.
8. Na página de **confirmação** , examine as configurações. Por padrão, a caixa de seleção **Adicionar todo o armazenamento qualificado para o cluster** é selecionada. Desmarque essa caixa de seleção se você quiser fazer um destes procedimentos:
    
      - Você deseja configurar o armazenamento mais tarde.
      - Você pretende criar espaços de armazenamento clusterizados por meio do Gerenciador de Cluster de Failover ou os cmdlets do PowerShell de Windows Clustering de Failover e ainda não tiver criado espaços de armazenamento no arquivo e serviços de armazenamento. Para obter mais informações, consulte [Implantar espaços de armazenamento clusterizados](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Selecione **Avançar** para criar o cluster de failover.
10. Na página de **Resumo** , confirme que o cluster de failover foi criado com êxito. Se houver quaisquer avisos ou erros, exiba o resumo de saída ou selecione **Exibir o relatório** para exibir o relatório completo. Selecione **Concluir**.
11. Para confirmar que o cluster foi criado, verifique se o nome do cluster é listado sob o **Gerenciador de Cluster de Failover** na árvore de navegação. Você pode expandir o nome do cluster e, em seguida, selecione itens em **nós**, **armazenamento** ou **redes** para exibir os recursos associados.
    
    Perceba que pode levar algum tempo para o nome do cluster replicar com êxito no DNS. Após o registro DNS bem-sucedida e replicação, se você selecionar **Todos os servidores** no Gerenciador do servidor, o nome do cluster deve ser listado como um servidor com um status de **capacidade de gerenciamento** de **Online**.

Depois que o cluster é criado, você pode fazer coisas como verificar a configuração de quorum de cluster e, opcionalmente, criar Volumes de compartilhados de Cluster (CSV). Para obter mais informações, consulte [Noções básicas sobre o Quorum em espaços de armazenamento diretos](../storage/storage-spaces/understand-quorum.md) e [Usar Volumes Compartilhados do Cluster em um cluster de failover](failover-cluster-csvs.md).

## Criar funções de cluster

Depois de criar o cluster de failover, você pode criar funções de cluster para cargas de trabalho de cluster de host.

>[!NOTE]
>Para funções de cluster que exigem um ponto de acesso do cliente, um objeto de computador virtual (VCO) é criado no AD DS. Por padrão, todos os VCOs para o cluster são criados no mesmo contêiner ou unidade Organizacional, como o CNO. Perceba que, depois de criar um cluster, você pode mover o CNO para qualquer UO.

Veja como criar uma função clusterizada:

1. Use o Gerenciador do servidor ou do Windows PowerShell para instalar a função ou recurso que é necessário para uma função em cluster em cada nó de cluster de failover. Por exemplo, se você quiser criar um servidor de arquivos em cluster, instale a função de servidor de arquivos em todos os nós de cluster.
    
    A tabela a seguir mostra as funções de cluster que você pode definir no Assistente para alta disponibilidade e a função de servidor associado ou recurso que você deve instalar como um pré-requisito.
    
    <table>
    <thead>
    <tr class="header">
    <th>Função clusterizada</th>
    <th>Função ou recurso pré-requisito</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Servidor de Namespace DFS</td>
    <td>Namespaces de DFS (parte da função de servidor de arquivos)</td>
    </tr>
    <tr class="even">
    <td>Servidor DHCP</td>
    <td>Função de servidor DHCP</td>
    </tr>
    <tr class="odd">
    <td>Coordenador de transações distribuídas (DTC)</td>
    <td>Nenhum(a)</td>
    </tr>
    <tr class="even">
    <td>Servidor de arquivos</td>
    <td>Função Servidor de Arquivos</td>
    </tr>
    <tr class="odd">
    <td>Aplicativo genérico</td>
    <td>Não aplicável</td>
    </tr>
    <tr class="even">
    <td>Script genérico</td>
    <td>Não aplicável</td>
    </tr>
    <tr class="odd">
    <td>Serviço genérico</td>
    <td>Não aplicável</td>
    </tr>
    <tr class="even">
    <td>Agente de réplica do Hyper-V</td>
    <td>Função Hyper-V</td>
    </tr>
    <tr class="odd">
    <td>iSCSI Target Server</td>
    <td>(parte da função de servidor de arquivos) do servidor de destino iSCSI</td>
    </tr>
    <tr class="even">
    <td>iSNS Server</td>
    <td>recurso de serviço do servidor iSNS</td>
    </tr>
    <tr class="odd">
    <td>Enfileiramento de mensagens</td>
    <td>Recurso de serviços de enfileiramento de mensagens de mensagem</td>
    </tr>
    <tr class="even">
    <td>Outro servidor</td>
    <td>Nenhum(a)</td>
    </tr>
    <tr class="odd">
    <td>Máquina virtual</td>
    <td>Função Hyper-V</td>
    </tr>
    <tr class="even">
    <td>Servidor WINS</td>
    <td>Recurso de servidor WINS</td>
    </tr>
    </tbody>
    </table>
2. No Gerenciador de Cluster de Failover, expanda o nome do cluster, clique com botão direito **funções**e, em seguida, selecione **Configurar função**.
3. Siga as etapas no Assistente para alta disponibilidade para criar a função clusterizada.
4. Para verificar se a função clusterizada foi criada, no painel de **funções** , certifique-se de que a função tem um status de **em execução**. O painel de funções também indica o nó proprietário. Para testar o failover, clique com botão direito a função, aponte para **Mover**e, em seguida, selecione **Selecionar nó**. Na caixa de diálogo **Mover função agrupadas** , selecione o nó do cluster desejado e, em seguida, selecione **Okey**. Na coluna **Nó proprietário** , verifique se o nó proprietário alterado.

## Criar um cluster de failover usando o Windows PowerShell

Os seguintes cmdlets do Windows PowerShell executar as mesmas funções que os procedimentos anteriores deste tópico. Insira cada cmdlet em uma única linha, mesmo que eles podem aparecer quebra de várias linhas devido a restrições de formatação.

>[!NOTE]
>Você deve usar o Windows PowerShell para criar um cluster desanexado do Active Directory no Windows Server 2012 R2. Para obter informações sobre a sintaxe, consulte [implantar um Cluster Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

O exemplo a seguir instala o recurso de Clustering de Failover.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

O exemplo a seguir executa todos os testes de validação de cluster em computadores que são nomeados *1* e *2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>O cmdlet **Test-Cluster** gera os resultados em um arquivo de log no diretório de trabalho atual. Por exemplo: \AppData\Local\Temp C:\Users\ < nome de usuário >.

O exemplo a seguir cria um cluster de failover que é chamado *meucluster* conosco *Server1* e *2*, atribui o de endereço IP estático *192.168.1.12*e adiciona todo o armazenamento qualificado para o cluster de failover.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

O exemplo a seguir cria o mesmo cluster de failover como no exemplo anterior, mas ele não adiciona armazenamento qualificado para o cluster de failover.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

O exemplo a seguir cria um cluster chamado *meucluster* no *Cluster* OU do domínio *Contoso.com*.

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

Para obter exemplos de como adicionar funções de cluster, consulte os tópicos como [Adicionar ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) e [Adicionar ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps).

## Mais informações

  - [Clustering de failover](failover-clustering.md)
  - [Implantar um Cluster Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Servidor de arquivos de escalabilidade horizontal para dados de aplicativo](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Implantar um Cluster desanexado diretório ativo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Usar o Clustering de Convidado para Alta Disponibilidade](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Atualização com Suporte a Cluster](cluster-aware-updating.md)
  - [Novo Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
