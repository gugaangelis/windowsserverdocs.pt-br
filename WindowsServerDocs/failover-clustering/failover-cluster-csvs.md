---
title: Use os Volumes de Cluster compartilhados em um cluster de failover
description: Como usar os Volumes de Cluster compartilhados em um cluster de failover.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f5bd0ad05bdc2573a5ea0abbe165de2d3e7f5c8f
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233512"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>Use os Volumes de Cluster compartilhados em um cluster de failover

>Aplica-se a: Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2016

Cluster compartilhados Volumes (CSV) permitem que vários nós em um cluster de failover simultaneamente ter acesso de leitura / gravação no mesmo LUN (disco) que é provisionado como um volume NTFS. (No Windows Server 2012 R2, o disco pode ser provisionado como NTFS ou o sistema de arquivos resiliente (ReFS).) Com CSV, funções clusterizadas podem failover rapidamente de um nó para outro nó sem exigir uma alteração na propriedade de unidade, ou desmontar e remontagem um volume. CSV também ajudar a simplificar o gerenciamento de um número potencialmente grande de LUNs em um cluster de failover.

CSV fornecem um sistema de arquivos em cluster finalidade geral, que é colocada em camadas acima NTFS (ou ReFS no Windows Server 2012 R2). Os aplicativos de CSV incluem:

- Agrupado arquivos do disco rígido virtual (VHD) para máquinas virtuais em cluster do Hyper-V
- Compartilhamentos de arquivos de dimensionamento para armazenar dados de aplicativo para a função de servidor de arquivos de dimensionamento agrupado. Arquivos de máquina virtual do Hyper-V e dados do Microsoft SQL Server são exemplos dos dados de aplicativo para essa função. (Lembre-se ReFS não é suportado para um servidor de arquivos de dimensionamento.) Para obter mais informações sobre o servidor de arquivos de dimensionamento, consulte [Servidor de arquivos de dimensionamento para dados de aplicativo](sofs-overview.md).

>[!NOTE]
>CSV não oferece suporte a carga de trabalho em cluster do Microsoft SQL Server no SQL Server 2012 e versões anteriores do SQL Server.

No Windows Server 2012, a funcionalidade CSV foi aprimorada significativamente. Por exemplo, dependências de serviços de domínio do Active Directory foram removidas. Suporte foi adicionado para as melhorias funcionais no **chkdsk**para a interoperabilidade com aplicativos antivírus e de backup e para integração com recursos de armazenamento geral como volumes de disco BitLocker criptografadas e espaços de armazenamento. Para obter uma visão geral da funcionalidade CSV que foi introduzida no Windows Server 2012, consulte [What's New no cluster de Failover no Windows Server 2012 \[redirected\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

Windows Server 2012 R2 introduz funcionalidades adicionais, como a propriedade CSV distribuída, maior resiliência por meio de disponibilidade do serviço do servidor, maior flexibilidade na quantidade de memória física que você pode alocar para o cache CSV, melhor diagnosibility e interoperabilidade avançada que inclua suporte à ReFS e eliminação da duplicação. Para obter mais informações, consulte [What's New no cluster de Failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

>[!NOTE]
>Para obter informações sobre o uso de eliminação da duplicação de dados CSV para cenários de Virtual Desktop Infrastructure (VDI), consulte que postagens de blog [eliminação da duplicação de dados de implantação para armazenamento VDI no Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) e [estender a eliminação da duplicação de dados ao novas cargas de trabalho em Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx).

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>Examinar os requisitos e considerações sobre o uso de CSV em um cluster de failover

Antes de usar CSV em um cluster de failover, examine a rede, armazenamento e outros requisitos e considerações nesta seção.

### <a name="network-configuration-considerations"></a>Considerações de configuração de rede

Considere o seguinte ao configurar as redes que oferecem suporte a CSV.

- **Vários adaptadores de rede e de várias redes**. Para habilitar a tolerância a falhas no caso de falha de rede, recomendamos que várias redes de cluster transportam o tráfego CSV ou que você configure parceria adaptadores de rede.
    
    Se os nós do cluster estiverem conectados a redes que não devem ser usados pelo cluster, desabilite-los. Por exemplo, é recomendável que você desative as redes iSCSI para uso do cluster impedir que o tráfego de CSV dessas redes. Para desabilitar uma rede, em Gerenciador de Cluster de Failover, selecione **redes**, selecione a rede, selecione a ação de **Propriedades** e, em seguida, selecione **não permita a comunicação de rede de cluster nesta rede**. Como alternativa, você pode configurar a propriedade da **função** da rede usando o cmdlet [Get-ClusterNetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) do Windows PowerShell.
- **Propriedades do adaptador de rede**. Nas propriedades de todos os adaptadores que executam comunicações de cluster, certifique-se de que as configurações a seguir estão habilitadas:

  - **Cliente para redes Microsoft** e **arquivos e impressoras para redes Microsoft de compartilhamento**. Essas configurações suportam SMB Server Message Block () 3.0, que é usado por padrão para transportar CSV tráfego entre os nós. Para habilitar SMB, também garantir que o serviço do servidor e o serviço de estação de trabalho estão em execução e se eles estão configurados para ser iniciado automaticamente em cada nó do cluster.

    >[!NOTE]
    >No Windows Server 2012 R2, existem várias instâncias de serviço do servidor por um nó de cluster de failover. Não há a instância padrão que processa o tráfego de entrada de clientes SMB que acessam os compartilhamentos de arquivo regular e uma segunda instância CSV que trata apenas o nó entre CSV tráfego. Além disso, se o serviço do servidor em um nó se tornar não íntegro, propriedade CSV automaticamente muda para outro nó.

    SMB 3.0 inclui os recursos de SMB multicanais e direta de SMB, que permitem o tráfego de CSV fluxo em várias redes do cluster e aproveitar os adaptadores de rede que suportam acesso direto de memória remoto (RDMA). Por padrão, o SMB multicanais é usado para tráfego CSV. Para obter mais informações, consulte [Visão geral do Server Message Block](../storage/file-server/file-server-smb-overview.md).
  - **Filtro de desempenho do Cluster de Failover do Microsoft adaptador Virtual**. Essa configuração melhora a capacidade de nós ao executar o redirecionamento de e/s quando ele é necessário para alcançar CSV, por exemplo, quando uma falha de conectividade impede que um nó se conectando diretamente ao disco CSV. Para obter mais informações, consulte [sincronização sobre e/s e o redirecionamento de e/s em comunicação CSV](#about-i/o-synchronization-and-i/o-redirection-in-csv-communication) posteriormente neste tópico.
- **Priorização de rede do cluster**. Geralmente, é recomendável que você não altere as preferências de cluster configurado para as redes.
- **Configuração da sub-rede IP**. Nenhuma configuração da sub-rede específica é necessária para nós em uma rede que usam CSV. CSV pode oferecer suporte a clusters com várias sub-redes.
- **Qualidade de serviço (QoS) baseada em política**. Recomendamos que você configure uma política de QoS prioridade e uma política de largura de banda mínima para tráfego de rede para cada nó quando você usa CSV. Para obter mais informações, consulte [Qualidade de serviço (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>).
- **Rede de armazenamento**. Para obter recomendações de rede de armazenamento, examine as diretrizes fornecidas pelo seu fornecedor de armazenamento. Para obter considerações adicionais sobre o armazenamento para CSV, consulte [requisitos de configuração de armazenamento e disco](#storage-and-disk-configuration-requirements) posteriormente neste tópico.

Para obter uma visão geral do hardware, rede e os requisitos de armazenamento para clusters de failover, consulte [Opções de armazenamento e requisitos de Hardware de cluster de Failover](clustering-requirements.md).

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>Sobre a sincronização de e/s e o redirecionamento de e/s em comunicação CSV

- **Sincronização de e/s**: CSV habilita vários nós a ter acesso de leitura-gravação simultâneo para o mesmo armazenamento compartilhado. Quando um nó realiza a entrada/saída de disco (e/s) em um volume CSV, o nó se comunica diretamente com o armazenamento, por exemplo, através de uma rede de área de armazenamento (SAN). No entanto, a qualquer momento, um único nó (chamado o nó coordenador) "é proprietário" recurso de disco físico que está associado com o LUN. O nó de coordenador para um volume CSV é exibido no Gerenciador de Cluster de Failover como **Proprietário do nó** em **discos**. Ele também aparece na saída do cmdlet [Get-ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) do Windows PowerShell.

  >[!NOTE]
  >No Windows Server 2012 R2, a propriedade CSV é distribuída uniformemente entre os nós do cluster de failover com base no número de volumes CSV proprietário de cada nó. Além disso, a propriedade automaticamente é rebalanceada quando houver condições como CSV failover, um nó ingressa novamente no cluster, você adicionar um novo nó ao cluster, você reiniciar um nó de cluster ou iniciar o cluster de failover após ele foi desligado.

  Quando determinadas pequenas alterações ocorrem no sistema de arquivos em um volume CSV, esses metadados devem ser sincronizados em cada um de nós físicos que acessam o LUN, e não apenas no nó coordenador único. Por exemplo, quando uma máquina virtual em um volume CSV é iniciada, criada ou excluída ou quando uma máquina virtual é migrada, essa informação precisa ser sincronizado em cada um de nós físicos que acessam a máquina virtual. Essas operações de atualização de metadados ocorrerem em paralelo nas redes de cluster usando SMB 3.0. Essas operações não exigem que todos os nós físicos para se comunicar com o armazenamento compartilhado.

- **Redirecionamento de e/s**: falhas de conectividade de armazenamento e determinadas operações de armazenamento podem impedir que um determinado nó se comunicam diretamente com o armazenamento. Para manter a função enquanto o nó não está se comunicando com o armazenamento, o nó redireciona o e/s de disco através de uma rede de cluster para o nó coordenador onde o disco está montado atualmente. Se o nó atual do coordenador sofrer uma falha de conectividade de armazenamento, todas as operações de e/s de disco são enfileiradas temporariamente enquanto um novo nó é estabelecido como um nó de coordenador.

O servidor usa um dos modos de redirecionamento e/s seguintes, dependendo da situação:

- **Redirecionamento de sistema de arquivo** O redirecionamento é por volume — por exemplo, quando CSV instantâneos são criados por um aplicativo de backup quando um volume CSV manualmente é colocado no modo de e/s redirecionado.
- **Redirecionamento do bloco** O redirecionamento é a nível de bloqueio de arquivos — por exemplo, quando se perder a um volume a conectividade de armazenamento. Redirecionamento de bloqueio é consideravelmente menor do que o redirecionamento de sistema de arquivo.

No Windows Server 2012 R2, você pode exibir o estado de um volume CSV em uma base por nó. Por exemplo, você pode ver se a e/s é direto ou redirecionadas ou se o volume CSV não está disponível. Se um volume CSV estiver em modo de e/s redirecionado, também é possível exibir o motivo. Use o cmdlet **Get-ClusterSharedVolumeState** do Windows PowerShell para exibir essas informações.

>[!NOTE]
> * No Windows Server 2012, devido aos aprimoramentos no design CSV, CSV executar operações de mais no modo de e/s direto que ocorre no Windows Server 2008 R2.
> * Devido a integração do CSV com recursos de SMB 3.0 como SMB multicanais e direta de SMB, o tráfego de e/s redirecionado pode transmitir em várias redes de cluster.
> * Você deve planejar suas redes de cluster para permitir o potencial aumento no tráfego de rede para o nó coordenador durante o redirecionamento de e/s.

### <a name="storage-and-disk-configuration-requirements"></a>Requisitos de configuração de armazenamento e disco

Para usar CSV, do armazenamento e dos discos devem atender aos seguintes requisitos:

- **Formato de arquivo do sistema**. No Windows Server 2012 R2, um espaço de armazenamento em disco ou para um volume CSV deve ser um disco básico que é particionado com NTFS ou ReFS. No Windows Server 2012, um espaço de armazenamento em disco ou para um volume CSV deve ser um disco básico que é particionado com NTFS.

  Um CSV tem os seguintes requisitos adicionais:
    
  - No Windows Server 2012 R2, você não pode usar um disco para um CSV que está formatado com FAT ou FAT32.
  - No Windows Server 2012, é possível usar um disco para um CSV que está formatado com FAT, FAT32 ou ReFS.
  - Se você quiser usar um espaço de armazenamento para um CSV, você pode configurar um espaço simple ou um espaço de espelho. No Windows Server 2012 R2, você também pode configurar um espaço de paridade. (No Windows Server 2012, CSV não suporta espaços paridade.)
  - Um CSV não pode ser usado como um disco de quórum de testemunha. Para obter mais informações sobre o quorum de cluster, consulte [Noções básicas sobre o Quorum em espaços de armazenamento direto](../storage/storage-spaces/understand-quorum.md).
  - Depois de adicionar um disco de CSV, ele será designado no formato CSVFS (para o sistema de arquivos CSV). Isso permite que o cluster e outros softwares para diferenciar o armazenamento CSV a partir de outros NTFS ou armazenamento ReFS. Em geral, CSVFS oferece suporte a mesma funcionalidade que ReFS ou NTFS. No entanto, alguns recursos não são suportados. Por exemplo, no Windows Server 2012 R2, não será possível habilitar compactação nas CSV. No Windows Server 2012, não será possível habilitar a compactação em CSV ou eliminação da duplicação de dados.
- **Tipo de recurso no cluster**. Para um volume CSV, você deve usar o tipo de recurso de disco físico. Por padrão, um espaço de armazenamento em disco ou que é adicionado ao armazenamento de cluster é automaticamente configurado dessa forma.
- **Discos de escolha de CSV ou outros discos no armazenamento de cluster**. Ao escolher um ou mais discos para uma máquina virtual em cluster, considere como cada disco será usado. Se um disco será usado para armazenar os arquivos que são criados por Hyper-V, como arquivos VHD ou arquivos de configuração, você pode escolher entre os discos CSV ou outros discos disponíveis no armazenamento do cluster. Se um disco será um disco físico que seja diretamente anexados à máquina virtual (também chamada de um disco de passagem), você não pode escolher um disco CSV e você deve escolher entre os discos disponíveis no armazenamento do cluster.
- **Nome do caminho para identificar os discos**. Discos em CSV são identificados com um nome de caminho. Cada caminho parece estar na unidade do sistema do nó como um volume numerado sob a pasta **\\ClusterStorage** . Esse caminho é o mesmo quando visualizado em qualquer nó do cluster. Você pode renomear os volumes, se necessário.

Requisitos de armazenamento para CSV, examine as diretrizes fornecidas pelo seu fornecedor de armazenamento. Para considerações de planejamento de armazenamento adicional para CSV, consulte [planeja usar CSV em um cluster de failover](#plan-to-use-csv-in-a-failover-cluster) mais adiante neste tópico.

### <a name="node-requirements"></a>Requisitos do nó

Para usar CSV, os nós devem atender aos seguintes requisitos:

- A **letra da unidade da disco do sistema**. Em todos os nós, a letra da unidade para o disco do sistema deve ser o mesmo.
- **Protocolo de autenticação**. O protocolo NTLM deve ser ativado em todos os nós. Isso é ativado por padrão.

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>Planeje usar CSV em um cluster de failover

Esta seção lista as considerações de planejamento e recomendações para usar CSV em um cluster de failover executando o Windows Server 2012 R2 ou Windows Server 2012.

>[!IMPORTANT]
>Peça ao seu fornecedor de armazenamento para obter recomendações sobre como configurar a sua unidade de armazenamento específica para CSV. Se as recomendações do fornecedor de armazenamento diferem das informações neste tópico, use as recomendações do fornecedor de armazenamento.

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Organização dos LUNs, volumes e arquivos VHD

Para tornar o melhor uso do CSV para fornecer armazenamento para máquinas virtuais em cluster, é útil examinar a como você faria organiza os LUNs (discos) ao configurar servidores físicos. Quando você configura as máquinas virtuais correspondentes, tente organizar os arquivos VHD de forma semelhante.

Considere um servidor físico para o qual você faria organizar os discos e os arquivos da seguinte maneira:

- Arquivos do sistema, incluindo um arquivo de página, em um disco físico
- Arquivos de dados em outro disco físico

Para uma máquina virtual em cluster equivalente, você deve organizar os volumes e arquivos de forma semelhante:

- Arquivos do sistema, incluindo um arquivo de página, em um arquivo VHD em um CSV
- Arquivos de dados em um arquivo VHD no outro CSV

Se você adicionar outra máquina virtual, onde for possível, você deve manter a mesma organização para os VHDs nesta máquina virtual.

### <a name="number-and-size-of-luns-and-volumes"></a>Número e tamanho dos volumes e LUNs

Quando você planejar a configuração de armazenamento de um cluster de failover que usa CSV, considere as seguintes recomendações:

- Decidir quantos LUNs configurar, consulte seu fornecedor de armazenamento. Por exemplo, seu fornecedor de armazenamento pode recomendar que você configurar cada LUN com uma partição e coloca um volume CSV contidas nela.
- Não há nenhuma limitação para o número de máquinas virtuais que podem ser suportados em um único volume CSV. No entanto, você deve considerar o número de máquinas virtuais que você planeja ter no cluster e a carga de trabalho (operações de e/s por segundo) para cada máquina virtual. Considere os exemplos a seguir:

  - Uma organização é a implantação de máquinas virtuais que dará suporte a uma infraestrutura de área de trabalho virtual (VDI), que é uma carga de trabalho relativamente clara. O cluster usa o armazenamento de alto desempenho. O administrador de cluster, após consultar o fornecedor de armazenamento, decide colocar um número relativamente grande de máquinas virtuais por volume CSV.
  - Outra organização está implantando um grande número de máquinas virtuais que dará suporte a um aplicativo de uso intensivo de banco de dados, que é uma carga de trabalho mais pesada. O cluster usa armazenamento desempenho inferior. O administrador de cluster, após consultar o fornecedor de armazenamento, decide colocar um número relativamente pequeno das máquinas virtuais por volume CSV.
- Quando você planejar a configuração de armazenamento para uma máquina virtual específica, considere os requisitos de disco do aplicativo, serviço ou função que dará suporte a máquina virtual. Noções básicas sobre esses requisitos ajuda a evitar contenção de disco que pode resultar em um desempenho fraco. A configuração de armazenamento da máquina virtual bastante deve se parecer com a configuração de armazenamento que você deseja usar para um servidor físico que está executando o mesmo serviço, um aplicativo ou função. Para obter mais informações, consulte [disposição de LUNs, volumes e os arquivos VHD](#arrangement-of-luns,-volumes,-and-vhd-files) anteriormente neste tópico.

    Você também pode reduzir a contenção de disco fazendo com que o armazenamento com um grande número de discos rígidos independentes. Escolha seu hardware de armazenamento de acordo e consulte seu fornecedor para otimizar o desempenho do armazenamento de dados.
- Dependendo das suas cargas de trabalho do cluster e sua necessidade de operações de e/s, você pode considerar a configuração apenas uma porcentagem das máquinas virtuais para acessar cada LUN, enquanto outras máquinas virtuais não tem conectividade e em vez disso são dedicadas para operações de computação.

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>Adicionar um disco para CSV em um cluster de failover

O recurso CSV é habilitado por padrão no cluster de Failover. Para adicionar um disco para CSV, você deve adicionar um disco ao grupo de **Armazenamento disponível** do cluster (se ela já não estiver adicionada) e, em seguida, adicione o disco para CSV no cluster. Você pode usar o Gerenciador de Cluster de Failover ou os cmdlets do Windows PowerShell Failover Clusters para executar estes procedimentos.

### <a name="add-a-disk-to-available-storage"></a>Adicionar um disco ao armazenamento disponível

1. No Failover Cluster Manager, na árvore de console, expanda o nome do cluster e, em seguida, expanda **armazenamento**.
2. Clique com o botão **discos**e selecione **Adicionar disco**. Aparecerá uma lista mostrando os discos que podem ser adicionados para uso em um cluster de failover.
3. Selecione o disco ou discos que você deseja adicionar e selecione **Okey**.

    Os discos agora são atribuídos ao grupo de **Armazenamento disponível** .

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Comandos do Windows PowerShell equivalentes (Adicionar um disco ao armazenamento disponível)

O seguinte cmdlet do Windows PowerShell ou cmdlets executar a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles podem aparecer a quebra de várias linhas aqui devido a restrições de formatação.

O exemplo a seguir identifica os discos que estão prontos para ser adicionado ao cluster e, em seguida, adiciona-los ao grupo de **Armazenamento disponível** .

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>Adicionar um disco no armazenamento disponível para CSV

1. No Failover Cluster Manager, na árvore de console, expanda o nome do cluster, expanda **armazenamento**e selecione **discos**.
2. Selecione um ou mais discos que estão atribuídos ao **Armazenamento disponível**, clique com o botão da seleção e selecione **Adicionar à Volumes Compartilhados do Cluster**.

    Os discos agora são atribuídos ao grupo de **Volume compartilhado do Cluster** do cluster. Os discos são expostos para cada nó do cluster como volumes numerados (pontos de montagem) sob a pasta % SystemDisk % ClusterStorage. Os volumes aparecem no sistema de arquivos CSVFS.

>[!NOTE]
>Você pode renomear volumes CSV na pasta % SystemDisk % ClusterStorage.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Comandos do Windows PowerShell equivalentes (Adicionar um disco para CSV)

O seguinte cmdlet do Windows PowerShell ou cmdlets executar a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles podem aparecer a quebra de várias linhas aqui devido a restrições de formatação.

O exemplo a seguir adiciona o *Cluster disco 1* em **Armazenamento disponível** para CSV no cluster local.

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>Habilitar o cache CSV para leitura intensiva de cargas de trabalho (opcional)

O cache CSV fornece armazenamento em cache no nível do bloco de operações de e/s sem buffer somente leitura, a alocação de memória de sistema (RAM) como um cache de write-through. (As operações de e/s sem buffer não estejam armazenadas pelo Gerenciador de cache.) Isso pode melhorar o desempenho para aplicativos como o Hyper-V, o qual realiza operações de e/s sem buffer ao acessar um VHD. O cache CSV pode melhorar o desempenho de solicitações de leitura sem cache solicitações de gravação. Habilitação do cache de CSV também é útil para cenários de servidor de arquivos de dimensionamento.

>[!NOTE]
>Recomendamos que você habilite o cache CSV para clusterizados todas as implantações de Hyper-V e o servidor de arquivos de dimensionamento.

Por padrão no Windows Server 2012, o cache CSV está desabilitado. No Windows Server 2012 R2, o cache CSV é habilitado por padrão. No entanto, você ainda deve alocar o tamanho do cache de bloqueio para reservar.

A tabela a seguir descreve as definições de configuração de dois que controlam o cache CSV.

<table>
<thead>
<tr class="header">
<th>Nome da propriedade no Windows Server 2012 R2</th>
<th>Nome da propriedade no Windows Server 2012</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>BlockCacheSize</strong></td>
<td><strong>SharedVolumeBlockCacheSizeInMB</strong></td>
<td>Esta é uma propriedade de comuns de cluster que permite que você defina a quantidade de memória (em megabytes) para reservar para o cache CSV em cada nó do cluster. Por exemplo, se um valor de 512 estiver definido, 512 MB de memória do sistema é reservado em cada nó. (Na maioria dos clusters, 512 MB é um valor recomendado.) A configuração padrão é 0 (para desabilitado).</td>
</tr>
<tr class="even">
<td><strong>EnableBlockCache</strong></td>
<td><strong>CsvEnableBlockCache</strong></td>
<td>Esta é uma propriedade particular do recurso de disco físico cluster. Ele permite que você habilitar o cache de CSV em um disco individual que é adicionado à CSV. No Windows Server 2012, a configuração padrão é 0 (para desabilitado). Para habilitar o cache CSV em um disco, configure um valor de 1. Por padrão, no Windows Server 2012 R2, essa configuração estiver habilitada.</td>
</tr>
</tbody>
</table>

Você pode monitorar o cache CSV no Monitor de desempenho, adicionando os contadores de **Cache de Volume de CSV de Cluster**.

#### <a name="configure-the-csv-cache"></a>Configurar o cache CSV

1. Inicie o Windows PowerShell como administrador.
2. Para definir um cache de *512* MB a ser reservado em cada nó, digite o seguinte:

    - Para o Windows Server 2012 R2:

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - Para o Windows Server 2012:

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. No Windows Server 2012, para habilitar o cache de CSV em um CSV chamado *1 de disco de Cluster*, digite o seguinte:

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * No Windows Server 2012, é possível alocar apenas 20% do total de RAM física no cache de CSV. No Windows Server 2012 R2, é possível alocar até 80%. Como os servidores de arquivo de dimensionamento não são geralmente memória restringida, você pode realizar ganhos de desempenho grande usando a memória extra para o cache CSV.
> * Para evitar a contenção de recursos, você deve reiniciar cada nó do cluster, depois de modificar a memória que é alocada para o cache CSV. No Windows Server 2012 R2, uma reinicialização não é mais necessária.
> * Depois de habilitar ou desabilitar o cache CSV em um disco individual, para que a configuração tenha efeito, você deve colocar o recurso de disco físico offline e colocá-lo novamente online. (Por padrão, no Windows Server 2012 R2, o cache CSV está habilitado.) 
> * Para obter mais informações sobre o cache CSV que inclui informações sobre contadores de desempenho, consulte o postagem no blog [como habilitar o Cache de CSV](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/).

## <a name="back-up-csv"></a>Fazer backup de CSV

Há vários métodos para fazer backup de informações armazenadas em CSV em um cluster de failover. Você pode usar um aplicativo de backup da Microsoft ou de um aplicativo não-Microsoft. Em geral, CSV impõe requisitos de backup especiais além daquelas para armazenamento em cluster formatado com NTFS ou ReFS. Backups CSV também não interromper outras operações de armazenamento CSV.

Quando você seleciona um aplicativo de backup e a agenda de backup para CSV, você deve considerar os seguintes fatores:

- Nível de volume de backup de um volume CSV pode ser executado de qualquer nó que se conecta ao volume CSV.
- Seu aplicativo de backup pode usar instantâneos de software ou os instantâneos de hardware. Dependendo da capacidade do seu aplicativo de backup para oferecer suporte a eles, backups podem usar instantâneos do serviço de cópia de sombra de Volume (VSS) consistente com o aplicativo e consistente com falha.
- Se você estiver fazendo backup CSV que possuem várias máquinas virtuais em execução, você geralmente deve escolher um método de backup baseado no sistema operacional gerenciamento. Se seu aplicativo de backup lhe fornecer apoio, várias máquinas virtuais podem fazer backup simultaneamente.
- CSV suportam os solicitantes de backup que estão executando o Windows Server 2012 R2 Backup, Backup do Windows Server 2012 ou Windows Server 2008 R2 Backup. No entanto, o Backup do Windows Server geralmente fornece apenas uma solução básica de backup que pode não ser adequada para organizações com clusters maiores. Backup do Windows Server não suporta o backup de máquina de consistente com o aplicativo virtual em CSV. Suporte a backup consistente após falhas em nível de volume somente. (Se você restaurar um backup consistente com falha, a máquina virtual será no mesmo estado era se a máquina virtual tinha paralisado no momento exato que o backup foi feito.) Um backup de uma máquina virtual em um volume CSV terá êxito, mas um evento de erro será registrado indicando que isso não é suportado.
- Ao fazer backup de um cluster de failover, você talvez precise credenciais administrativas.

>[!IMPORTANT]
>Certifique-se de revisar cuidadosamente quais dados o seu aplicativo de backup faz o backup e restaura, quais recursos CSV oferece suporte, e os requisitos de recursos para o aplicativo em cada nó do cluster.

>[!WARNING]
>Se você precisar restaurar os dados de backup em um volume CSV, lembre-se das capacidades e limitações do aplicativo de backup para manter e restaurar dados consistente com o aplicativo em todos os nós do cluster. Por exemplo, com alguns aplicativos, se o CSV é restaurado em um nó diferente do nó onde o volume CSV submetido a backup, você pode sobrescrever inadvertidamente dados importantes sobre o estado do aplicativo no nó onde está ocorrendo a restauração.

## <a name="more-information"></a>Mais informações

- [Clustering de failover](failover-clustering.md)
- [Implantar espaços de armazenamento em cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)