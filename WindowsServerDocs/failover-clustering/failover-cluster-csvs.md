---
title: Usar volumes compartilhados de cluster em um cluster de failover
description: Como usar volumes compartilhados de cluster em um cluster de failover.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: da0f541c34c7f8687822bec365364fdd406fa3c3
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370701"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>Usar volumes compartilhados de cluster em um cluster de failover

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Os CSVs (Volumes Compartilhados Clusterizados) habilitam múltiplos nós em um cluster de failover a ter acesso de leitura e gravação simultaneamente ao LUN (número de unidade lógica), ou disco, provisionado como volume NTFS (New Technology File System). (No Windows Server 2012 R2, o disco pode ser provisionado como NTFS ou ReFS (sistema de arquivos resilientes).) Com o CSV, as funções clusterizadas podem fazer failover rapidamente de um nó para outro nó sem a necessidade de uma alteração na propriedade da unidade, ou desmontagem e remontagem de um volume. O CSV também ajuda a simplificar o gerenciamento potencial de muitos LUNs em um cluster de failover.

O CSV fornece um sistema de arquivos clusterizado de uso geral, que é colocado em camadas acima do NTFS (ou ReFS no Windows Server 2012 R2). Os aplicativos do CSV incluem:

- Arquivos VHD (disco rígido virtual) clusterizados para máquinas virtuais clusterizadas do Hyper-V
- Compartilhamentos escaláveis de arquivos para armazenar dados de aplicativo para a função clusterizada Servidor de Arquivos Escalável. Exemplos dos dados de aplicativo dessa função incluem arquivos de máquina virtual do Hyper-V e dados do Microsoft SQL Server. (Lembre-se de que ReFS não tem suporte para um Servidor de Arquivos de Escalabilidade Horizontal.) Para obter mais informações sobre Servidor de Arquivos de Escalabilidade Horizontal, consulte [servidor de arquivos de escalabilidade horizontal para dados de aplicativos](sofs-overview.md).

> [!NOTE]
> CSVs não dão suporte à carga de trabalho clusterizada Microsoft SQL Server no SQL Server 2012 e versões anteriores do SQL Server.

No Windows Server 2012, a funcionalidade CSV foi significativamente aprimorada. Por exemplo, foram removidas as dependências nos Serviços de Domínio Active Directory. Foi adicionado suporte às melhorias funcionais do **chkdsk** para interoperabilidade com aplicativos antivírus e de backup, além de integração com recursos de armazenamento gerais, como volumes criptografados com o BitLocker e Espaços de Armazenamento. Para obter uma visão geral da funcionalidade CSV introduzida no Windows Server 2012, consulte [novidades no clustering de failover no Windows server 2012 \[\]Redirecionado ](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

O Windows Server 2012 R2 apresenta funcionalidade adicional, como a propriedade de CSV distribuída, maior resiliência por meio da disponibilidade do serviço de servidor, maior flexibilidade na quantidade de memória física que você pode alocar para o cache CSV, melhor diagnosibility e a interoperabilidade aprimorada que inclui suporte para ReFS e eliminação de duplicação. Para obter mais informações, consulte [novidades no clustering de failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

> [!NOTE]
> Para obter informações sobre o uso de eliminação de duplicação de dados em CSV para cenários de Virtual Desktop Infrastructure (VDI), consulte as postagens de blog [Implantação de eliminação de duplicação de dados para o armazenamento VDI no Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) e [Estendendo a duplicação de dados para novas cargas de trabalho no Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx).

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>Revisar requisitos e considerações para usar o CSV em um cluster de failover

Antes de usar o CSV em um cluster de failover, revise os requisitos e considerações de rede, armazenamento, e outros, indicados nesta seção.

### <a name="network-configuration-considerations"></a>Considerações de configuração de rede

Considere o seguinte ao configurar redes que deem suporte o CSV.

- **Múltiplas redes e múltiplos adaptadores de rede**. Para habilitar a tolerância a falhas em caso de falha de rede, recomendamos que as redes com múltiplos clusters transportem o tráfego CSV ou que os adaptadores de rede agrupados sejam configurados.
    
    Se os nós de cluster estiverem conectados a redes que não devam ser usadas pelo cluster, desabilite-os. Por exemplo, recomendamos desabilitar redes iSCSI (Internet Small Computer System Interface) para uso de cluster, a fim de impedir o tráfego CSV nessas redes. Para desabilitar uma rede, em Gerenciador de Cluster de Failover, selecione **redes**, selecione a rede, selecione a ação **Propriedades** e, em seguida, selecione **não permitir comunicação de rede de cluster nesta rede**. Como alternativa, você pode configurar a propriedade **role** da rede usando o cmdlet [Get-ClusterNetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) do Windows PowerShell.
- **Propriedades do adaptador de rede**. Verifique se as configurações a seguir estão habilitadas nas propriedades de todos os adaptadores de rede que transportarem a comunicação do cluster:

  - **Cliente para redes Microsoft** e **Compartilhamento Arquivos/Impressoras para Redes Microsoft**. Essas configurações dão suporte ao protocolo SMB 3.0, que é usado por padrão para transportar o tráfego CSV entre os nós. Para habilitar o SMB, certifique-se também de que o serviço Servidor e Estação de Trabalho estejam sendo executados e configurados para serem iniciados automaticamente em cada nó de cluster.

    >[!NOTE]
    >No Windows Server 2012 R2, há várias instâncias de serviço de servidor por nó de cluster de failover. Há a instância padrão, que manuseia o tráfego de entrada dos clientes SMB que acessam compartilhamentos de arquivos regulares, e uma segunda instância do CSV, que manuseia somente o tráfego CSV entre nós. Além disso, se o serviço Servidor em um nó tiver sua integridade comprometida, a propriedade de CSV mudará automaticamente para outro nó.

    O SMB 3.0 inclui os recursos SMB Multichannel e SMB Direct, que habilitam a transmissão do tráfego CSV em várias redes no cluster e o aproveitamento de adaptadores de rede que deem suporte ao o RDMA (Acesso Remoto Direto à Memória). Por padrão, o SMB Multichannel é usado para o tráfego CSV. Para saber mais, confira [Visão geral do protocolo SMB](../storage/file-server/file-server-smb-overview.md).
  - **Filtro de desempenho do adaptador virtual de cluster de failover da Microsoft**. Esta configuração aprimora a capacidade de os nós efetuarem o redirecionamento de E/S, quando for preciso se comunicar ao CSV. Por exemplo, quando uma falha de conectividade impedir que o nó se conecte diretamente ao disco do CSV. Para obter mais informações, consulte [sobre sincronização de e/s e redirecionamento de e/s na comunicação CSV,](#about-io-synchronization-and-io-redirection-in-csv-communication) mais adiante neste tópico.
- **Priorização de rede de cluster**. Geralmente, é recomendável não alterar as preferências configuradas no cluster para as redes.
- **Configuração da sub-rede de IP**. Nenhuma configuração de sub-rede específica é necessária para que os nós de uma rede usem o CSV. O CSV pode dar suporte a clusters com múltiplas sub-redes.
- **QoS (Qualidade de Serviço) baseado em políticas**. Recomendamos configurar uma política de prioridade de QoS e uma política de largura de banda mínima para o tráfego de rede em cada nó ao usar o CSV. Para obter mais informações, consulte [Quality of Service (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>).
- **Rede de armazenamento**. Para recomendações de rede de armazenamento, analise as diretrizes fornecidas pelo seu fornecedor de armazenamento. Para obter considerações adicionais sobre o armazenamento para CSV, consulte [requisitos de configuração de armazenamento e disco](#storage-and-disk-configuration-requirements) mais adiante neste tópico.

Para uma visão geral dos requisitos de hardware, rede e armazenamento para os clusters de failover, confira [Requisitos de hardware de clustering de failover e opções de armazenamento](clustering-requirements.md).

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>Sobre a sincronização de E/S e redirecionamento de E/S na comunicação do CSV

- **Sincronização de e/s**: o CSV permite que vários nós tenham acesso de leitura/gravação simultâneo ao mesmo armazenamento compartilhado. Quando um nó realizar uma entrada/saída (E/S) de disco em um volume CSV, o nó se comunicará diretamente com o armazenamento, por meio se uma SAN (rede de área de armazenamento), por exemplo. Contudo, a todo momento, um único nó (chamado nó coordenador) é “dono” do recurso Disco Físico associado ao LUN. O nó coordenador de um volume CSV é exibido no Gerenciador de Cluster de Failover como **Nó do Proprietário**, em **Discos**. Ele também aparece na saída do cmdlet [Get-ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) do Windows PowerShell.

  >[!NOTE]
  >No Windows Server 2012 R2, a propriedade CSV é distribuída uniformemente entre os nós de cluster de failover com base no número de volumes CSV que cada nó possui. Além disso, a propriedade é rebalanceada automaticamente em caso de condições, como failover de CSV, um nó reingressar no cluster, adição de um novo nó ao cluster, reinicialização de um nó de cluster ou inicialização do cluster de failover após um desligamento.

  Quando certas alterações pequenas ocorrerem no sistema de arquivos de um volume CSV, esses metadados deverão ser sincronizados em cada um dos nós físicos que acessarem o LUN, não somente no nó coordenador único. Por exemplo, quando uma máquina virtual em um volume CSV for iniciada, criada ou excluída, ou quando uma máquina virtual for migrada, tais informações precisarão ser sincronizadas em cada um dos nós físicos que acessarem a máquina virtual. Tais operações de atualização de metadados ocorrem em paralelo nas redes de cluster, usando o SMB 3.0. Essas operações não requerem que todos os nós físicos se comuniquem com o armazenamento compartilhado.

- **Redirecionamento de e/s**: falhas de conectividade de armazenamento e determinadas operações de armazenamento podem impedir que um determinado nó se comunique diretamente com o armazenamento. Para manter as funções enquanto o nó não se comunicar com o armazenamento, o nó redirecionará, com uma rede de cluster, a E/S do disco para o nó coordenador em que o disco estiver montado. Se o nó coordenador atual enfrentar uma falha de conectividade com o armazenamento, todas as operações de E/S de disco serão temporariamente enfileiradas enquanto o novo nó for estabelecido como coordenador.

O servidor usa um dos seguintes modos de redirecionamento de E/S, dependendo da situação:

- **Redirecionamento do sistema de arquivos** O redirecionamento ocorre por volume. Por exemplo, quando os instantâneos de CSV são obtidos por um aplicativo de backup com a colocação manual de um volume CSV no modo de E/S redirecionada.
- **Redirecionamento de bloco** O redirecionamento ocorre no nível do bloco de arquivos. Por exemplo, quando a conectividade de um volume com o armazenamento é perdida. O redirecionamento de bloco é significativamente mais rápido do que o redirecionamento do sistema de arquivos.

No Windows Server 2012 R2, você pode exibir o estado de um volume CSV em uma base por nó. Você pode, por exemplo, ver se a E/S é direta ou redirecionada, ou se o volume CSV está indisponível. Se um volume CSV estiver no modo de E/S redirecionada, você também poderá exibir o motivo. Use o cmdlet **Get-ClusterSharedVolumeState** do Windows PowerShell para ver essas informações.

> [!NOTE]
> * No Windows Server 2012, devido a melhorias no design de CSV, o CSV executa mais operações no modo de e/s direto do que ocorreu no Windows Server 2008 R2.
> * Pela integração do CSV com os recursos do SMB 3.0, como SMB Multichannel e SMB Direct, o tráfego de E/S redirecionada pode fluir em diversas redes de cluster.
> * Planeje suas redes de cluster para permitir um potencial aumento no tráfego de rede para o nó coordenador durante o redirecionamento de E/S.

### <a name="storage-and-disk-configuration-requirements"></a>Requisitos de configuração de disco e armazenamento

Para usar o CSV, seu armazenamento e discos precisam cumprir os seguintes requisitos:

- **Formato do sistema de arquivos**. No Windows Server 2012 R2, um espaço de disco ou de armazenamento para um volume CSV deve ser um disco básico particionado com NTFS ou ReFS. No Windows Server 2012, um disco ou espaço de armazenamento para um volume CSV deve ser um disco básico particionado com NTFS.

  Um CSV tem os seguintes requisitos adicionais:
    
  - No Windows Server 2012 R2, você não pode usar um disco para um CSV formatado com FAT ou FAT32.
  - No Windows Server 2012, você não pode usar um disco para um CSV formatado com FAT, FAT32 ou ReFS.
  - Se desejar usar um espaço de armazenamento para um CSV, você poderá configurar um espaço simples ou de espelho. No Windows Server 2012 R2, você também pode configurar um espaço de paridade. (No Windows Server 2012, o CSV não oferece suporte a espaços de paridade.)
  - Um CSV não pode ser usado como disco testemunha de quorum. Para obter mais informações sobre o quorum do cluster, consulte [Understanding quorum in espaços de armazenamento diretos](../storage/storage-spaces/understand-quorum.md).
  - Depois de adicionar um disco como CSV, ele é designado no formato CSVFS (de “sistema de arquivos CSV”). Isso permite que o cluster e outros softwares diferenciem o armazenamento do CSV de outros armazenamentos NTFS ou ReFS. Em geral, o CSVFS dá suporte às mesmas funcionalidades que o NTFS ou ReFS. Porém, certos recursos não têm suporte. Por exemplo, no Windows Server 2012 R2, você não pode habilitar a compactação em CSV. No Windows Server 2012, não é possível habilitar a eliminação de duplicação ou a compactação de dados em CSV.
- **Tipo de recurso no cluster**. Para um volume CSV, é necessário usar o tipo de recurso Disco Físico. Por padrão, um disco ou espaço de armazenamento adicionado ao armazenamento de cluster é automaticamente configurado assim.
- **Escolha de discos do CSV ou outros discos no armazenamento de cluster**. Ao escolher um ou mais discos para uma máquina virtual clusterizada, considere como cada disco será usado. Se um disco for usado para armazenar arquivos criados pelo Hyper-V, tal como arquivos VHD ou de configuração, você pode escolher entre os discos CSV ou outros discos disponíveis no armazenamento de cluster. Se o disco for um disco físico conectado diretamente à máquina virtual (também chamado de disco de passagem), você não poderá escolher um disco do CSV e deverá escolher outro entre os discos disponíveis no armazenamento de cluster.
- **Nome de caminho para identificação dos discos**. Os discos no CSV são identificados com um nome de caminho. Cada caminho parece estar na unidade do sistema do nó como um volume numerado na pasta **\\ClusterStorage** . Esse caminho é o mesmo visto de qualquer nó do cluster. Você poderá renomear os volumes se necessário.

Para ver os requisitos de armazenamento do CSV, analise as diretrizes fornecidas pelo seu fornecedor de armazenamento. Para considerações de planejamento de armazenamento adicionais para o CSV, confira [Planejar o uso do CSV em um cluster de failover](#plan-to-use-csv-in-a-failover-cluster) posteriormente neste tópico.

### <a name="node-requirements"></a>Requisitos de nó

Para usar o CSV, seus nós precisam cumprir os seguintes requisitos:

- **Letra da unidade de disco do sistema**. Em todos os nós, a letra da unidade do disco do sistema deve ser a mesma.
- **Protocolo de autenticação**. O protocolo NTLM deve estar habilitado em todos os nós. Isso está habilitado por padrão.

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>Planejar o uso do CSV em um cluster de failover

Esta seção lista as considerações de planejamento e recomendações para usar o CSV em um cluster de failover que executa o Windows Server 2012 R2 ou o Windows Server 2012.

> [!IMPORTANT]
> Pergunte ao seu fornecedor de armazenamento quais são as recomendações para configurar sua unidade de armazenamento específica para o CSV. Se as recomendações informadas pelo fornecedor de armazenamento forem diferentes das informações indicadas neste tópico, use as recomendações do fornecedor.

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Organização dos LUNs, volumes e arquivos VHD

Para aproveitar ao máximo o CSV para oferecer armazenamento para máquinas virtuais, é recomendável analisar como você deseja organizar os LUNs (discos) ao configurar os servidores físicos. Ao configurar as máquinas virtuais correspondentes, tente organizar os arquivos VHD de maneira semelhante.

Considere um servidor físico no qual você organizaria os discos e arquivos da seguinte maneira:

- Arquivos do sistema, incluindo um arquivo de paginação, em um disco físico
- Arquivos de dados em outro disco físico

Para uma máquina virtual clusterizada equivalente, organize os volumes e arquivos de maneira semelhante:

- Arquivos do sistema, incluindo um arquivo de paginação, em um arquivo VHD em um CSV
- Arquivos de dados em um arquivo VHD em outro CSV

Se você adicionar outra máquina virtual, sempre que possível, mantenha os VHDs organizados da mesma forma nessa outra máquina.

### <a name="number-and-size-of-luns-and-volumes"></a>Número e tamanho dos LUNs e volumes

Ao planejar a configuração de armazenamento para um cluster de failover que usar o CSV, considere as seguintes recomendações:

- Para decidir quantos LUNs devem ser configurados, consulte seu fornecedor de armazenamento. Por exemplo seu fornecedor de armazenamento pode recomendar configurar cada LUN com uma partição e colocar um volume CSV nela.
- Não há limites para o número de máquinas virtuais que pode ter suporte em um único volume CSV. Contudo, você deve considerar o número de máquinas virtuais que planeja ter no cluster e a carga de trabalho (operações de E/S por segundo) de cada máquina virtual. Considere os exemplos a seguir:

  - Uma organização está implantando máquinas virtuais que darão suporte a uma VDI, uma carga de trabalho relativamente leve. O cluster usa um armazenamento de alto desempenho. O administrador do cluster, após consultar o fornecedor de armazenamento, decide colocar um número relativamente grande de máquinas virtuais por volume CSV.
  - Outra organização está implantando um grande número de máquinas virtuais que darão suporte a um aplicativo de banco de dados altamente usado, uma carga de trabalho mais pesada. O cluster usa um armazenamento de baixo desempenho. O administrador do cluster, após consultar o fornecedor de armazenamento, decide colocar um número relativamente pequeno de máquinas virtuais por volume CSV.
- Ao planejar a configuração de armazenamento de uma máquina virtual específica, considere os requisitos de disco do serviço, aplicativo ou função aos quais a máquina virtual dará suporte. Compreender esses requisitos ajuda a evitar uma contenção de disco, resultando em baixo desempenho. A configuração de armazenamento da máquina virtual deve assemelhar-se à configuração de armazenamento usada para um servidor físico executando o mesmo serviço, aplicativo ou função. Para obter mais informações, consulte [disposição de LUNs, volumes e arquivos VHD](#arrangement-of-luns-volumes-and-vhd-files) anteriormente neste tópico.

    Você também pode mitigar a contenção de disco ao ter um armazenamento com grande número de discos rígidos físicos independentes. Escolha o hardware de armazenamento adequadamente e consulte seu fornecedor para saber como otimizar o desempenho do armazenamento.
- Dependendo das cargas de trabalho de cluster e da necessidade de operações de E/S das cargas, você pode considerar configurar somente um percentual das máquinas virtuais para acessar cada LUN, enquanto as outras não teriam conectividade e seriam dedicadas a operações de computação.

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>Adicionar um disco ao CSV em um cluster de failover

O recurso CSV é habilitado por padrão no Clustering de Failover. Para adicionar um disco ao CSV, é necessário adicioná-lo ao grupo **Armazenamento Disponível** do cluster (se já não tiver sido adicionado) e depois adicioná-lo ao CSV no cluster. Você pode usar os cmdlets Gerenciador de Cluster de Failover ou clusters de failover do Windows PowerShell para executar esses procedimentos.

### <a name="add-a-disk-to-available-storage"></a>Adicionar um disco ao armazenamento disponível

1. No Gerenciador de Cluster de Failover, na árvore de console, expanda o nome do cluster e expanda **Armazenamento**.
2. Clique com o botão direito do mouse em **discos**e selecione **adicionar disco**. É exibida uma lista mostrando os discos que podem ser adicionados para uso no cluster de failover.
3. Selecione o disco ou os discos que você deseja adicionar e, em seguida, selecione **OK**.

    Os discos serão atribuídos ao grupo **Armazenamento Disponível**.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Comandos equivalentes do Windows PowerShell (adicionar um disco ao armazenamento disponível)

O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.

O exemplo a seguir identifica os discos já adicionados ao cluster e adiciona-os ao grupo **Armazenamento Disponível**.

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>Adicionar um disco no armazenamento disponível ao CSV

1. No Gerenciador de Cluster de Failover, na árvore de console, expanda o nome do cluster, expanda **armazenamento**e, em seguida, selecione **discos**.
2. Selecione um ou mais discos atribuídos ao **armazenamento disponível**, clique com o botão direito do mouse na seleção e selecione **Adicionar aos volumes compartilhados do cluster**.

    Os discos serão atribuídos ao grupo **Volume Compartilhado do Cluster** no cluster. Os discos são expostos para cada nó de cluster como volumes numerados (pontos de montagem) na pasta %SystemDisk%ClusterStorage. Os volumes aparecem no sistema de arquivos CSVFS.

>[!NOTE]
>Você pode renomear os volumes CSV na pasta %SystemDisk%ClusterStorage.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Comandos equivalentes do Windows PowerShell (adicionar um disco ao CSV)

O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.

O exemplo a seguir adiciona o *disco de cluster 1*, no **Armazenamento Disponível**, ao CSV no cluster local.

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>Habilitar o cache do CSV para cargas de trabalho de leitura intensa (opcional)

O cache do CSV oferece armazenamento em cache no nível do bloco das operações de E/S somente leitura não armazenadas em buffer alocando a memória do sistema (RAM) como um cache de gravação. (As operações de e/s sem buffer não são armazenadas em cache pelo Gerenciador de cache.) Isso pode melhorar o desempenho de aplicativos como o Hyper-V, que realiza operações de e/s sem buffer ao acessar um VHD. O cache do CSV pode melhorar o desempenho de solicitações de leitura sem solicitações de gravação de armazenamento em cache. Habilitar o cache do CSV também é útil para cenários do Servidor de Arquivos Escalável.

>[!NOTE]
>Recomendamos habilitar o cache do CSV para todas as implantações clusterizadas do Hyper-V e do Servidor de Arquivos Escalável.

Por padrão, no Windows Server 2012, o cache CSV é desabilitado. No Windows Server 2012 R2 e posterior, o cache CSV é habilitado por padrão. Porém, você ainda deverá alocar o tamanho do cache de bloco para a reserva.

A tabela a seguir descreve as duas definições de configuração que controlam o cache do CSV.

| Windows Server 2012 R2 e posterior |  Windows Server 2012                 | Descrição |
| -------------------------------- | ------------------------------------ | ----------- |
| BlockCacheSize                   | SharedVolumeBlockCacheSizeInMB       | Esta é uma propriedade de cluster comum que permite definir quanta memória (em megabytes) será reservada para o cache do CSV em cada nó do cluster. Por exemplo, ao definir o valor 512, 512 MB da memória do sistema serão reservados em cada nó. (Em muitos clusters, 512 MB é um valor recomendado.) A configuração padrão é 0 (para desabilitado). |
| EnableBlockCache                 | CsvEnableBlockCache                  | Esta é uma propriedade privada do recurso Disco Físico do cluster. Ela permite habilitar o cache do CSV em um disco individual adicionado ao CSV. No Windows Server 2012, a configuração padrão é 0 (para desabilitado). Para habilitar o cache de CSV em um disco, configure um valor de 1. Por padrão, no Windows Server 2012 R2, essa configuração é habilitada. |

Você pode monitorar o cache do CSV no Monitor de Desempenho adicionando os contadores em **Cache de Volume CSV de Cluster**.

#### <a name="configure-the-csv-cache"></a>Configurar o cache CSV

1. Inicie o Windows PowerShell como administrador.
2. Para definir um cache de *512* MB a ser reservado em cada nó, digite o seguinte:

    - Para o Windows Server 2012 R2 e posterior:

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - Para o Windows Server 2012:

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. No Windows Server 2012, para habilitar o cache CSV em um CSV denominado *disco 1 do cluster*, insira o seguinte:

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * No Windows Server 2012, você pode alocar apenas 20% da RAM física total para o cache CSV. No Windows Server 2012 R2 e posterior, você pode alocar até 80%. Como os Servidores de Arquivos Escaláveis geralmente não são restritos pela memória, você pode obter maiores ganhos de desempenho usando a memória extra para o cache do CSV.
> * Para evitar a contenção de recursos, você deve reiniciar cada nó no cluster depois de modificar a memória alocada para o cache CSV. No Windows Server 2012 R2 e posterior, uma reinicialização não é mais necessária.
> * Depois de habilitar ou desabilitar o cache do CSV em um disco específico, deixe o recurso Disco Físico offline e deixe-o novamente online para que a configuração entre em vigor. (Por padrão, no Windows Server 2012 R2 e posterior, o cache CSV está habilitado.) 
> * Para obter mais informações sobre o cache CSV que inclui informações sobre contadores de desempenho, consulte a postagem de blog [Como habilitar o cache de CSV](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/).

## <a name="backing-up-csvs"></a>Fazendo backup de CSVs

Há vários métodos para fazer backup de informações armazenadas em CSVs em um cluster de failover. Você pode usar um aplicativo de backup que seja ou não da Microsoft. Em geral, o CSV não tem requisitos especiais de backup além dos usuais para armazenamento clusterizado formatado com NTFS ou ReFS. Os backups do CSV também não interrompem outras operações de armazenamento do CSV.

Considere os seguintes fatores ao escolher um aplicativo e agenda de backup para o CSV:

- O backup no nível do volume de um volume CSV pode ser executado de qualquer nó conectado ao volume CSV.
- Seu aplicativo de backup pode usar instantâneos de software ou hardware. Dependendo da capacidade do aplicativo de backup para dar suporte a eles, os backups podem usar instantâneos do VSS (Serviço de Cópias de Sombra de Volume) consistentes com aplicativos ou falhas.
- Se estiver fazendo backup de um CSV contendo diversas máquinas virtuais em execução, você deverá escolher um método de backup baseado no sistema operacional de gerenciamento. Se o aplicativo de backup der suporte esta função, será possível efetuar o backup de diversas máquinas virtuais ao mesmo tempo.
- O CSV dá suporte a solicitantes de backup que executam o backup do Windows Server 2012 R2, o backup do Windows Server 2012 ou o backup do Windows Server 2008 R2. Porém, o Backup do Windows Server geralmente fornece apenas uma solução de backup básica que pode não ser adequada a organizações com clusters grandes. O Backup do Windows Server não dá suporte a backup de máquinas virtuais consistentes com aplicativos no CSV. Ele dá suporte somente a backup no nível do volume consistente com falhas. (Se você restaurar um backup consistente com falhas, a máquina virtual estará no mesmo estado em que estava se a máquina virtual tivesse falhado no momento exato em que o backup foi feito.) Um backup de uma máquina virtual em um volume CSV terá sucesso, mas um evento de erro será registrado indicando que não há suporte para isso.
- Você pode precisar de credenciais administrativas ao fazer backup de um cluster de failover.

> [!IMPORTANT]
>Certifique-se de analisar atentamente os dados que o aplicativo de backup salvar e restaurar, a quais recursos do CSV ele dá suporte e os requisitos de recursos para o aplicativo em cada nó de cluster.

> [!WARNING]
> Se você precisar restaurar os dados do backup em um volume CSV, esteja ciente das capacidades e limitações do aplicativo de backup quanto a manter e restaurar dados consistentes com aplicativos em nós de cluster. Por exemplo, com alguns aplicativos, se o CSV for restaurado em um nó diferente do qual backup do volume CSV tiver sido efetuado, você poderá inadvertidamente substituir dados importantes sobre o estado do aplicativo no nó da restauração.

## <a name="more-information"></a>Mais informações

- [Clustering de failover](failover-clustering.md)
- [Implantar espaços de armazenamento em cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)