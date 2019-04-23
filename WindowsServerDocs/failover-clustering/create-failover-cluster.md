---
title: criar um cluster de failover
description: Como criar um cluster de failover para o Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f919e69488c4f2272ddd07e535ba4e2248ddf79c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843287"
---
# <a name="create-a-failover-cluster"></a>criar um cluster de failover

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Este tópico mostra como criar um cluster de failover usando o snap-in Gerenciador de Cluster de Failover ou o Windows PowerShell. O tópico aborda uma implantação típica, na qual os objetos de computador do cluster e suas funções clusterizadas associadas são criados no AD DS (Serviços de Domínio Active Directory). Se você estiver implantando um cluster de espaços de armazenamento diretos, em vez disso, consulte [implantar espaços de armazenamento diretos](../storage/storage-spaces/deploy-storage-spaces-direct.md).

Você também pode implantar um cluster desanexado do Active Directory. Este método de implantação permite criar um cluster de failover sem permissões para criar objetos de computador no AD DS ou sem a necessidade de solicitar que objetos de computador sejam pré-configurados no AD DS. Essa opção só é disponibilizada pelo Windows PowerShell e é recomendável apenas para cenários específicos. Para obter mais informações, consulte [Implantar um Cluster Desanexado do Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### <a name="checklist-create-a-failover-cluster"></a>Lista de verificação: criar um cluster de failover

|Status|Tarefa|Referência|
|:---:|---|---|
|☐|Verificar os pré-requisitos|[Verifique se os pré-requisitos](#verify-the-prerequisites)|
|☐|Instalar o recurso de Clustering de Failover em todos os servidores que você deseja adicionar como um nó de cluster|[Instalar o recurso de Clustering de Failover](#install-the-failover-clustering-feature)|
|☐|Executar o Assistente de Validação de cluster para validar a configuração|[Validar a configuração](#validate-the-configuration)|
|☐|Executar o Assistente para Criação de Clusters para criar o cluster de failover|[Criar o cluster de failover](#create-the-failover-cluster)|
|☐|Criar funções clusterizadas para as cargas de trabalho do cluster do host|[Criar funções clusterizadas](#create-clustered-roles)|

## <a name="verify-the-prerequisites"></a>Verificar os pré-requisitos

Antes de começar, verifique os seguintes pré-requisitos:

- Verifique se todos os servidores que você deseja adicionar aos nós de cluster estão executando a mesma versão do Windows Server.
- Examine os requisitos de hardware para garantir que sua configuração seja aceita. Para obter mais informações, consulte [Requisitos de hardware de clustering de failover e opções de armazenamento](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Se você estiver criando um cluster de espaços de armazenamento diretos, consulte [requisitos de hardware de espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Para adicionar armazenamento clusterizado durante a criação do cluster, certifique-se de que todos os servidores podem acessar o armazenamento. (Você também poderá adicionar armazenamento clusterizado depois de criar o cluster.)
- Verifique se todos os servidores que você deseja adicionar como nós de cluster ingressaram no mesmo domínio do Active Directory.
- (Opcional) Crie uma OU (unidade organizacional) e mova as contas de computador para os servidores que deseja adicionar como nós de cluster à OU. Como melhor prática, é recomendável posicionar os clusters de failover na própria OU do AD DS. Isso pode ajudar a controlar as configurações de Política de Grupo ou configurações de modelo de segurança que afetarão os nós de cluster. Isolar os clusters na própria OU também ajuda a impedir a exclusão acidental de objetos de computador do cluster.

Além disso, verifique os seguintes requisitos de conta:

- Verifique se a conta que você deseja usar para criar o cluster é de um usuário do domínio que tenha direitos de administrador em todos os servidores que você deseja adicionar como nós de cluster.
- Verifique se qualquer uma destas opções é verdadeira:
    - O usuário que cria o cluster tem permissão para **Criar Objetos de Computador** para a OU ou o contêiner no qual residem os servidores que formarão o cluster.
    - Se o usuário não tiver permissão para **Criar Objetos de Computador**, peça para um administrador do domínio pré-configurar um objeto de computador para o cluster. Para obter mais informações, consulte [Pré-configurar objetos de computador de cluster nos Serviços de Domínio Active Directory](prestage-cluster-adds.md).

>[!NOTE]
>Esse requisito não se aplica se você quiser criar um cluster desanexado do Active Directory no Windows Server 2012 R2. Para obter mais informações, consulte [Implantar um Cluster Desanexado do Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## <a name="install-the-failover-clustering-feature"></a>Instalar o recurso de Clustering de Failover

Você deve instalar o recurso de Clustering de Failover em todos os servidores que deseja adicionar como nós de cluster de failover.

### <a name="install-the-failover-clustering-feature"></a>Instalar o recurso de Clustering de Failover

1. Inicie o Gerenciador do Servidor.
2. Sobre o **Manage** menu, selecione **adicionar funções e recursos**.
3. Sobre o **antes de começar** página, selecione **próxima**.
4. Sobre o **Selecionar tipo de instalação** , selecione **instalação baseada em função ou recurso**e, em seguida, selecione **próxima**.
5. Sobre o **Selecionar servidor de destino** , selecione o servidor no qual você deseja instalar o recurso e, em seguida, selecione **próxima**.
6. Sobre o **selecionar funções de servidor** , selecione **próxima**.
7. Na página **Selecionar recursos**, marque a caixa de seleção **Clustering de Failover.**
8. Para instalar as ferramentas de gerenciamento de cluster de failover, selecione **adicionar recursos**e, em seguida, selecione **próxima**.
9. Sobre o **confirmar seleções de instalação** página, selecione **instalar**.
<br>A reinicialização do servidor não é necessária para o recurso de Clustering de Failover.

10. Quando a instalação for concluída, selecione **fechar**.
11. Repita esse procedimento em todos os servidores que deseja adicionar como nós de cluster de failover.

>[!NOTE]
>Depois que você instalar o recurso de Clustering de Failover, será recomendável aplicar as últimas atualizações do Windows Update. Além disso, para um cluster de failover com base no Windows Server 2012, examine os [hotfixes e atualizações recomendados para clusters de failover com base no Windows Server 2012](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) Microsoft Support do artigo e instale todas as atualizações aplicáveis.

## <a name="validate-the-configuration"></a>Validar a configuração

Antes que você crie o cluster de failover, é expressamente recomendável validar a configuração para garantir que as configurações de hardware e software sejam compatíveis com o clustering de failover. A Microsoft dará suporte a uma solução de cluster apenas se a configuração completa for aprovada em todos os testes de validação e se todo o hardware estiver certificado para a versão do Windows Server que os nós de cluster estão executando.

>[!NOTE]
>Você deve ter pelo menos dois nós para realizar todos os testes. Se você tiver apenas um nó, muitos dos testes de armazenamento críticos não serão realizados.

### <a name="run-cluster-validation-tests"></a>Executar testes de validação de cluster

1. Em um computador que tenha Ferramentas de Gerenciamento de Cluster de Failover instaladas a partir das Ferramentas de Administração de Servidor Remoto ou em um servidor no qual você tenha instalado o recurso de Clustering de Failover, inicie o Gerenciador de Cluster de Failover. Para fazer isso em um servidor, inicie o Gerenciador do servidor e, em seguida, o **ferramentas** menu, selecione **Gerenciador de Cluster de Failover**.
2. No **Gerenciador de Cluster de Failover** painel, em **Management**, selecione **validar configuração**.
3. Sobre o **antes de começar** página, selecione **próxima**.
4. No **selecionar servidores ou um Cluster** página, o **insira o nome do** caixa, digite o nome NetBIOS ou o nome de domínio totalmente qualificado de um servidor que você planeja adicionar como um nó de cluster de failover e, em seguida, selecione **Adicionar**. Repita essa etapa para cada servidor que quiser adicionar. Para adicionar vários servidores ao mesmo tempo, separe os nomes usando uma vírgula ou um ponto e vírgula. Por exemplo, digite os nomes no formato *server1.contoso.com, server2.contoso.com*. Quando tiver terminado, selecione **próxima**.
5. Sobre o **opções de teste** página, selecione **executar todos os testes (recomendados)** e, em seguida, selecione **próxima**.
6. Sobre o **confirmação** página, selecione **próxima**.

    A página Validando exibe o status dos testes em execução.
7. Na página **Resumo**, realize uma destas ações:
    
      - Se os resultados indicarem que os testes foram concluídos com êxito e a configuração é adequada ao clustering e você deseja criar o cluster imediatamente, verifique se o **criar o cluster agora usando os nós validados** verificar caixa está selecionada e, em seguida, selecione **concluir**. Em seguida, continue na etapa 4 do procedimento [Criar o cluster de failover](#create-the-failover-cluster).
      - Se os resultados indicarem que existem advertências ou falhas, selecione **Exibir relatório** para exibir os detalhes e determinar quais problemas devem ser corrigidos. Observe que uma advertência para um teste de validação específico indica que esse aspecto do cluster de failover pode ser aceito, mas pode não atender às melhores práticas recomendadas.
        
        >[!NOTE]
        >Se você receber um aviso para o teste Validar reserva persistente para espaços de armazenamento, consulte a postagem no blog [Aviso de validação de cluster de failover do Windows indica que seus discos não dão suporte a reservas persistentes para espaços de armazenamento](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) para mais informações.

Para obter mais informações sobre o inventário de hardware, consulte [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="create-the-failover-cluster"></a>Criar o cluster de failover

Para concluir esta etapa, verifique se a conta de usuário com a qual você fez logon atende aos requisitos destacados na seção [Verificar os pré-requisitos](#verify-the-prerequisites) deste tópico.

1. Inicie o Gerenciador do Servidor.
2. Sobre o **ferramentas** menu, selecione **Gerenciador de Cluster de Failover**.
3. No **Gerenciador de Cluster de Failover** painel, em **Management**, selecione **criar Cluster**.
    
    O Assistente para Criação de Clusters é aberto.
4. Sobre o **antes de começar** página, selecione **próxima**.
5. Se o **selecionar servidores** página for exibida, na **digite o nome** caixa, digite o nome NetBIOS ou o nome de domínio totalmente qualificado de um servidor que você planeja adicionar como um nó de cluster de failover e, em seguida, selecione **Adicionar**. Repita essa etapa para cada servidor que quiser adicionar. Para adicionar vários servidores ao mesmo tempo, separe os nomes usando vírgula ou ponto e vírgula. Por exemplo, digite os nomes no formato *server1.contoso.com; server2.contoso.com*. Quando tiver terminado, selecione **próxima**.
    
    >[!NOTE]
    >Se você optar por criar o cluster imediatamente após executar a validação na [Validando o procedimento de configuração](#validate-the-configuration), você não verá o **selecionar servidores** página. Os nós que foram validados são adicionados automaticamente ao Assistente de para Criação de Clusters para que você não precise inseri-los novamente.
6. Se você tiver ignorado a validação anteriormente, a página **Advertência de Validação** será exibida. É expressamente recomendável realizar a validação do cluster. Apenas os clusters que são aprovados em todos os testes de validação são aceitos pela Microsoft. Para executar os testes de validação, selecione **Yes**e, em seguida, selecione **próxima**. Conclua o Assistente de configuração de validação conforme descrito em [validar a configuração](#validate-the-configuration).
7. Na página **Ponto de Acesso para Administrar o Cluster**, faça o seguinte:
    
    1. Na caixa **Nome do Cluster**, digite o nome que deseja usar para administrar o cluster. Antes disso, examine as seguintes informações:
        
          - Durante a criação do cluster, esse nome é registrado como o objeto de computador do cluster (também conhecido como *CNO* ou *objeto de nome do cluster*) no AD DS. Se você especificar um nome NetBIOS para o cluster, o CNO será criado no mesmo local em que residem os objetos de computador dos nós do cluster. Isso pode ser o contêiner de computadores padrão ou uma OU.
          - Para especificar um local diferente para o CNO, você pode digitar o nome diferenciado de uma OU na caixa **Nome do Cluster** . Por exemplo:  *CN=ClusterName, OU=Clusters, DC=Contoso, DC=com*.
          - Se um administrador do domínio tiver pré-configurado o CNO em uma OU diferente da qual residem os nós do cluster, especifique o nome diferenciado fornecido pelo administrador do domínio.
    2. Se o servidor não tiver um adaptador de rede configurado para usar protocolo DHCP, você deverá configurar um ou mais endereços IP estáticos para o cluster de failover. Marque a caixa de seleção ao lado de cada rede que deseja usar para o gerenciamento de cluster. Selecione o **endereço** campo ao lado de uma rede selecionada e, em seguida, insira o endereço IP que você deseja atribuir ao cluster. Esse endereço (ou endereços) IP será associado ao nome do cluster no DNS (Sistema de Nomes de Domínio).
    3. Quando tiver terminado, selecione **próxima**.
8. Na página **Confirmação**, examine as configurações. Por padrão, a caixa de seleção **Adicione todo o armazenamento qualificado ao cluster** é marcada. Desmarque essa caixa de seleção se:
    
      - Quiser configurar o armazenamento depois.
      - Estiver planejando criar espaços de armazenamento clusterizados por meio do Gerenciador de Cluster de Failover ou pelos cmdlets de Clustering de Failover do Windows PowerShell, e ainda não tiver criado espaços de armazenamento nos Serviços de Arquivo e Armazenamento. Para obter mais informações, consulte [Deploy Clustered Storage Spaces (Implantar espaços de armazenamento clusterizados)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Selecione **próxima** para criar o cluster de failover.
10. Na página **Resumo**, confirme se o cluster de failover foi criado com sucesso. Se existirem advertências ou erros, exiba a saída do resumo ou selecione **Exibir relatório** para exibir o relatório completo. Selecione **concluir**.
11. Para confirmar se o cluster foi criado, verifique se o nome do cluster está listado no **Gerenciador de Cluster de Failover**, na árvore de navegação. Você pode expandir o nome do cluster e, em seguida, selecione itens em **nós**, **armazenamento** ou **redes** para exibir os recursos associados.
    
    Observe que poderá levar algum tempo para que o nome do cluster seja replicado com sucesso no DNS. Depois do registro DNS bem-sucedida e a replicação, se você selecionar **todos os servidores** no Gerenciador do servidor, o nome do cluster deve estar listado como um servidor com um **gerenciabilidade** status de **Online** .

Depois que o cluster é criado, você pode realizar ações como verificar a configuração do quorum do cluster e, opcionalmente, criar CSVs (Volumes Compartilhados Clusterizados). Para obter mais informações, consulte [Noções básicas sobre o Quorum em espaços de armazenamento diretos](../storage/storage-spaces/understand-quorum.md) e [usar Volumes Compartilhados do Cluster em um cluster de failover](failover-cluster-csvs.md).

## <a name="create-clustered-roles"></a>Criar funções clusterizadas

Depois de criar o cluster de failover, você pode criar funções clusterizadas para as cargas de trabalho do cluster do host.

>[!NOTE]
>Para funções clusterizadas que exigem um ponto de acesso para cliente, um VCO (objeto de computador virtual) é criado no AD DS. Por padrão, todos os VCOs do cluster são criados no mesmo contêiner ou OU do CNO. Observe que depois de criar um cluster, você poderá mover o CNO para qualquer OU.

Aqui está como criar uma função clusterizada:

1. Use o Gerenciador do Servidor ou o Windows PowerShell para instalar a função ou o recurso exigido para uma função clusterizada em cada nó de cluster de failover. Por exemplo, se quiser criar um servidor de arquivos clusterizado, instale a função Servidor de Arquivos em todos os nós do cluster.
    
    A tabela a seguir mostra as funções clusterizadas que você pode configurar no Assistente de Alta Disponibilidade e a função de servidor ou o recurso associado que você deve instalar como um pré-requisito.
    
    <table>
    <thead>
    <tr class="header">
    <th>Função clusterizada</th>
    <th>Função ou recurso de pré-requisito</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Servidor de namespace do DFS</td>
    <td>Namespaces do DFS (parte da função Servidor de Arquivos)</td>
    </tr>
    <tr class="even">
    <td>Servidor DHCP</td>
    <td>Função Servidor DHCP</td>
    </tr>
    <tr class="odd">
    <td>DTC (Coordenador de Transações Distribuídas):</td>
    <td>Nenhuma</td>
    </tr>
    <tr class="even">
    <td>Servidor de arquivos</td>
    <td>Função Servidor de Arquivos</td>
    </tr>
    <tr class="odd">
    <td>Aplicativo genérico</td>
    <td>Não Aplicável</td>
    </tr>
    <tr class="even">
    <td>Script genérico</td>
    <td>Não Aplicável</td>
    </tr>
    <tr class="odd">
    <td>Serviço genérico</td>
    <td>Não Aplicável</td>
    </tr>
    <tr class="even">
    <td>Agente de Réplica do Hyper-V</td>
    <td>Função Hyper-V</td>
    </tr>
    <tr class="odd">
    <td>iSCSI Target Server</td>
    <td>Servidor de Destino iSCSI (parte da função Servidor de Arquivos)</td>
    </tr>
    <tr class="even">
    <td>iSNS Server</td>
    <td>Recurso Serviço do iSNS Server</td>
    </tr>
    <tr class="odd">
    <td>Enfileiramento de Mensagens</td>
    <td>Recurso Serviços de Enfileiramento de Mensagens</td>
    </tr>
    <tr class="even">
    <td>Outro servidor</td>
    <td>Nenhuma</td>
    </tr>
    <tr class="odd">
    <td>Máquina Virtual</td>
    <td>Função Hyper-V</td>
    </tr>
    <tr class="even">
    <td>Servidor WINS</td>
    <td>Recurso servidor WINS</td>
    </tr>
    </tbody>
    </table>
2. No Gerenciador de Cluster de Failover, expanda o nome do cluster, clique com botão direito **funções**e, em seguida, selecione **configurar função**.
3. Siga as etapas do Assistente de Alta Disponibilidade para criar a função clusterizada.
4. Para verificar se a função clusterizada foi criada, no painel **Funções**, verifique se o status da função está definido como **Em execução**. O painel Funções também indica o nó do proprietário. Para testar o failover, a função com o botão direito, aponte para **mova**e, em seguida, selecione **selecionar nó**. No **mover função clusterizada** caixa de diálogo, selecione o nó de cluster desejado e, em seguida, selecione **Okey**. Na coluna **Nó do Proprietário** , verifique se o nó do proprietário foi alterado.

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>Criar um cluster de failover usando o Windows PowerShell

Os seguintes cmdlets do Windows PowerShell executar as mesmas funções que os procedimentos anteriores neste tópico. Insira cada cmdlet em uma única linha, embora eles possam aparecer com quebras automáticas em várias linhas devido às restrições de formatação.

>[!NOTE]
>Você deve usar o Windows PowerShell para criar um cluster desanexado do Active Directory no Windows Server 2012 R2. Para obter informações de sintaxe, consulte [Deploy an Active Directory-Detached Cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

O exemplo a seguir instala o recurso de Clustering de Failover.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

O exemplo a seguir realiza todos os testes de validação em computadores denominados como *Server1* e *Server2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>O **Test-Cluster** cmdlet gera os resultados em um arquivo de log no diretório de trabalho atual. Por exemplo:  C:\Users\<username > \AppData\Local\Temp.

O exemplo a seguir cria um cluster de failover denominado como *MyCluster* com os nós *Server1* e *Server2*, atribui o endereço IP estático *192.168.1.12* e adiciona todo o armazenamento qualificado ao cluster de failover.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

O exemplo a seguir cria o mesmo cluster de failover do exemplo anterior, mas não adiciona armazenamento qualificado a ele.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

O exemplo a seguir cria um cluster denominado como *MyCluster* na OU *Cluster* do domínio *Contoso.com*.

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

Para obter exemplos de como adicionar funções clusterizadas, consulte tópicos como [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) e [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps).

## <a name="more-information"></a>Mais informações

  - [Clustering de failover](failover-clustering.md)
  - [Implantar um Cluster do Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Servidor de arquivos de escalabilidade horizontal para dados de aplicativo](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Implantar um Cluster de desanexado do Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Using Guest Clustering for High Availability](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Cluster-Aware Updating](cluster-aware-updating.md)
  - [New-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Cluster de teste](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
