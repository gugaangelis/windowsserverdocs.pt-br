---
title: Conjuntos de cluster
ms.prod: windows-server
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: Este artigo descreve o cenário de conjuntos de clusters
ms.localizationpriority: medium
ms.openlocfilehash: 52d686fa9797d84f56182b15c36a26440792ec13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402920"
---
# <a name="cluster-sets"></a>Conjuntos de cluster

> Aplica-se a: Windows Server 2019

Conjuntos de clusters é a nova tecnologia de expansão de nuvem na versão 2019 do Windows Server que aumenta a contagem de nós de cluster em uma única nuvem do SDDC (Data Center) definida pelo software em ordens de magnitude. Um conjunto de clusters é um agrupamento livremente acoplado de vários clusters de failover: computação, armazenamento ou hiperconvergente. A tecnologia de conjuntos de clusters habilita a fluida da máquina virtual entre clusters de membros em um conjunto de clusters e um namespace de armazenamento unificado no conjunto em suporte ao fluido da máquina virtual.

Enquanto preserva as experiências de gerenciamento de cluster de failover existentes em clusters de membros, uma instância de conjunto de clusters também oferece casos de uso de chave sobre o gerenciamento do ciclo de vida na agregação. Este guia de avaliação do cenário do Windows Server 2019 fornece as informações básicas necessárias, juntamente com as instruções detalhadas para avaliar a tecnologia de conjuntos de clusters usando o PowerShell.

## <a name="technology-introduction"></a>Introdução à tecnologia

A tecnologia de conjuntos de clusters é desenvolvida para atender às solicitações específicas do cliente as nuvens do Data Center (SDDC) definidas pelo software operacional em escala. A proposta de valor de conjuntos de cluster pode ser resumida da seguinte maneira:  

- Aumente significativamente o dimensionamento de nuvem do SDDC com suporte para executar máquinas virtuais com alta disponibilidade combinando vários clusters menores em uma única malha grande, mesmo mantendo o limite de falha de software em um único cluster
- Gerenciar todo o ciclo de vida do cluster de failover, incluindo a integração e a desativação de clusters, sem afetar a disponibilidade da máquina virtual de locatário, por meio da migração fluida de máquinas virtuais nessa grande malha
- Alterar facilmente a proporção de computação para armazenamento em seu Hyper-convergente
- Beneficie-se de [domínios de falha do Azure e conjuntos de disponibilidade](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) entre clusters no posicionamento inicial da máquina virtual e na migração de máquina virtual subsequente
- Combine diferentes gerações de hardware de CPU na mesma malha de conjunto de clusters, mesmo mantendo os domínios de falha individuais homogêneos para máxima eficiência.  Observe que a recomendação do mesmo hardware ainda está presente em cada cluster individual, bem como no conjunto de clusters inteiro.

De uma exibição de alto nível, é assim que os conjuntos de clusters podem ser.

![Exibição de solução de conjuntos de clusters](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

O seguinte fornece um resumo rápido de cada um dos elementos na imagem acima:

**Cluster de gerenciamento**

O cluster de gerenciamento em um conjunto de clusters é um cluster de failover que hospeda o plano de gerenciamento altamente disponível de todo o conjunto de clusters e o Servidor de Arquivos de Escalabilidade Horizontal de referência (namespace de conjunto de cluster) de armazenamento unificado (SOFS). Um cluster de gerenciamento é logicamente dissociado de clusters de membros que executam as cargas de trabalho da máquina virtual. Isso torna o plano de gerenciamento do conjunto de clusters resiliente a quaisquer falhas localizadas em todo o cluster, por exemplo, a perda da potência de um cluster membro.   

**Cluster de membros**

Um cluster de membros em um conjunto de clusters é normalmente um cluster hiperconvergente tradicional executando máquinas virtuais e Espaços de Armazenamento Diretos cargas de trabalho. Os clusters de vários membros participam de uma única implantação de conjunto de clusters, formando a maior malha de nuvem do SDDC. Os clusters de membros diferem de um cluster de gerenciamento em dois aspectos principais: os clusters de membros participam de construções de domínio de falha e de conjunto de disponibilidade, e os clusters de membros também são dimensionados para hospedar máquinas virtuais e Espaços de Armazenamento Diretos cargas de trabalho. As máquinas virtuais de conjunto de clusters que se movem entre limites de cluster em um conjunto de clusters não devem ser hospedadas no cluster de gerenciamento por esse motivo.

**SOFS de referência do namespace do conjunto de clusters**

Uma referência de namespace de conjunto de clusters (namespace de conjunto de cluster) SOFS é uma Servidor de Arquivos de Escalabilidade Horizontal em que cada compartilhamento SMB no namespace do conjunto de clusters SOFS é um compartilhamento de referência – do tipo ' SimpleReferral ' recentemente introduzido no Windows Server 2019. Essa referência permite que clientes do protocolo SMB acessem o compartilhamento SMB de destino hospedado no SOFS do cluster de membros. O SOFS de referência do namespace do conjunto de clusters é um mecanismo de referência leve e, dessa forma, não participa do caminho de e/s. As referências de SMB são armazenadas em cache de forma perpétua em cada um dos nós de cliente e o namespace de conjuntos de clusters é atualizado dinamicamente automaticamente essas referências, conforme necessário.

**Mestre de conjunto de clusters**

Em um conjunto de clusters, a comunicação entre os clusters de membros é acoplada livremente e é coordenada por um novo recurso de cluster chamado "conjunto de cluster mestre" (CS-Master). Como qualquer outro recurso de cluster, o CS-Master é altamente disponível e resiliente a falhas de cluster de membros individuais e/ou falhas de nó de cluster de gerenciamento. Por meio de um novo provedor WMI de conjunto de clusters, CS-Master fornece o ponto de extremidade de gerenciamento para todas as interações de gerenciabilidade do conjunto

**Trabalho do conjunto de clusters**

Em uma implantação de conjunto de clusters, o CS-Master interage com um novo recurso de cluster nos clusters de membros chamado "conjunto de trabalho do cluster" (CS-Worker). CS-Worker atua como a única ligação no cluster para orquestrar as interações de cluster local conforme solicitado pelo CS-Master. Exemplos dessas interações incluem o posicionamento da máquina virtual e o inventário de recursos locais do cluster. Há apenas uma instância CS-Worker para cada um dos clusters de membro em um conjunto de clusters. 

**Domínio de falha**

Um domínio de falha é o agrupamento de artefatos de software e hardware que o administrador determina que pode falhar juntos quando ocorrer uma falha.  Embora um administrador possa designar um ou mais clusters juntos como um domínio de falha, cada nó poderia participar de um domínio de falha em um conjunto de disponibilidade. Os conjuntos de clusters por design deixam a decisão de determinação de limite de domínio de falha para o administrador que está bem inverso com data center considerações de topologia – por exemplo, PDU, rede – que os clusters de membros compartilham. 

**Conjunto de disponibilidade**

Um conjunto de disponibilidade ajuda o administrador a configurar a redundância desejada de cargas de trabalho clusterizadas entre domínios de falha, organizando-os em um conjunto de disponibilidade e implantando cargas de trabalho nesse conjunto de disponibilidade. Digamos que, se você estiver implantando um aplicativo de duas camadas, recomendamos que você configure pelo menos duas máquinas virtuais em um conjunto de disponibilidade para cada camada, o que garantirá que, quando um domínio de falha nesse conjunto de disponibilidade ficar inativo, seu aplicativo terá pelo menos uma máquina virtual em cada camada hospedada em um domínio de falha diferente desse mesmo conjunto de disponibilidade.

## <a name="why-use-cluster-sets"></a>Por que usar conjuntos de clusters

Os conjuntos de clusters fornecem o benefício da escala sem sacrificar a resiliência.  

Os conjuntos de clusters permitem o clustering de vários clusters em conjunto para criar uma malha grande, enquanto cada cluster permanece independente para resiliência.  Por exemplo, você tem vários clusters de HCI de 4 nós executando máquinas virtuais.  Cada cluster fornece a resiliência necessária para si mesmo.  Se o armazenamento ou a memória começar a ser preenchido, escalar verticalmente será a próxima etapa.  Com o dimensionamento, há algumas opções e considerações.

1. Adicione mais armazenamento ao cluster atual.  Com o Espaços de Armazenamento Diretos, isso pode ser complicado, pois exatamente as mesmas unidades de modelo/firmware podem não estar disponíveis.  A consideração dos tempos de recriação também precisa ser levada em conta.
2. Adicione mais memória.  E se você estiver se esgotando na memória que os computadores podem manipular?  E se todos os slots de memória disponíveis estiverem cheios?
3. Adicione nós de computação adicionais com unidades ao cluster atual.  Isso nos leva de volta à opção 1 que precisa ser considerada.
4. Comprar um novo cluster inteiro

É aí que os conjuntos de clusters fornecem o benefício do dimensionamento.  Se eu adicionar meus clusters a um conjunto de cluster, posso aproveitar o armazenamento ou a memória que pode estar disponível em outro cluster sem nenhuma compra adicional.  De uma perspectiva de resiliência, a adição de nós adicionais a uma Espaços de Armazenamento Diretos não fornecerá votos adicionais para o quorum.  Como mencionado [aqui](drive-symmetry-considerations.md), um cluster espaços de armazenamento diretos pode sobreviver à perda de dois nós antes de ficar inativo.  Se você tiver um cluster de HCI de 4 nós, os 3 nós ficarão inativos levarão todo o cluster.  Se você tiver um cluster de 8 nós, 3 nós desaparecerão o cluster inteiro.  Com conjuntos de clusters que têm dois clusters de HCI de 4 nós no conjunto, 2 nós em um HCI ficam inativos e um nó no outro HCI fica inativo, ambos os clusters permanecem ativos.  É melhor criar um grande cluster de Espaços de Armazenamento Diretos de 16 nós ou dividi-lo em quatro clusters de 4 nós e usar conjuntos de clusters?  Ter quatro clusters de 4 nós com conjuntos de clusters oferece a mesma escala, mas maior resiliência no fato de que vários nós de computação podem ficar inativos (inesperadamente ou para manutenção) e a produção permanece.

## <a name="considerations-for-deploying-cluster-sets"></a>Considerações sobre a implantação de conjuntos de clusters

Ao considerar se os conjuntos de clusters são algo que você precisa usar, considere estas perguntas:

- Você precisa ir além dos limites atuais de computação e escala de armazenamento do HCI?
- Toda a computação e o armazenamento não são idênticos ao mesmo?
- Você faz a migração dinâmica de máquinas virtuais entre clusters?
- Você gostaria de definir conjuntos de disponibilidade do computador do Azure e domínios de falha em vários clusters?
- Você precisa levar algum tempo para examinar todos os seus clusters para determinar onde as novas máquinas virtuais precisam ser colocadas?

Se sua resposta for sim, os conjuntos de clusters serão o que você precisa.

Há alguns outros itens a serem considerados onde um SDDC maior pode alterar suas estratégias de data center geral.  SQL Server é um bom exemplo.  Mover SQL Server máquinas virtuais entre clusters requer que o licenciamento SQL seja executado em nós adicionais?  

## <a name="scale-out-file-server-and-cluster-sets"></a>Servidor de arquivos de escalabilidade horizontal e conjuntos de cluster

No Windows Server 2019, há uma nova função de servidor de arquivos de escalabilidade horizontal chamada Servidor de Arquivos de Escalabilidade Horizontal de infraestrutura (SOFS). 

As seguintes considerações se aplicam a uma função de infraestrutura SOFS:

1.  Pode haver no máximo uma função de cluster SOFS de infraestrutura em um cluster de failover. A função SOFS de infraestrutura é criada especificando o parâmetro de opção " **-Infrastructure**" para o cmdlet **Add-ClusterScaleOutFileServerRole** .  Por exemplo:

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Cada volume CSV criado no failover aciona automaticamente a criação de um compartilhamento SMB com um nome gerado automaticamente com base no nome do volume CSV. Um administrador não pode criar ou modificar diretamente compartilhamentos SMB em uma função SOFS, exceto por operações de criação/modificação de volume CSV.

3.  Em configurações hiperconvergentes, um SOFS de infraestrutura permite que um cliente SMB (host Hyper-V) se comunique com a AC (disponibilidade contínua garantida) para a infraestrutura SOFS servidor SMB. Essa AC de loopback de SMB hiperconvergente é obtida por meio de máquinas virtuais que acessam seus arquivos VHDx (disco virtual) em que a identidade proprietária da máquina virtual é encaminhada entre o cliente e o servidor. Esse encaminhamento de identidade permite arquivos de VHDx da ACL, assim como nas configurações de cluster hiperconvergentes padrão como antes.

Depois que um conjunto de clusters é criado, o namespace do conjunto de clusters depende de uma infraestrutura SOFS em cada um dos clusters Membros e, além disso, de uma infraestrutura SOFS no cluster de gerenciamento.

No momento em que um cluster de membros é adicionado a um conjunto de cluster, o administrador especifica o nome de uma infraestrutura SOFS nesse cluster, se já existir uma. Se a infraestrutura SOFS não existir, uma nova função SOFS de infraestrutura no novo cluster de membros será criada por essa operação. Se uma função de SOFS de infraestrutura já existir no cluster de membros, a operação de adição o renomeará implicitamente para o nome especificado, conforme necessário. Quaisquer servidores SMB singleton existentes ou funções de SOFS de não infraestrutura nos clusters de membros são deixados não utilizados pelo conjunto de clusters. 

No momento em que o conjunto de clusters é criado, o administrador tem a opção de usar um objeto de computador do AD já existente como a raiz do namespace no cluster de gerenciamento. As operações de criação do conjunto de clusters criam a função de cluster SOFS de infraestrutura no cluster de gerenciamento ou renomeia a função SOFS de infraestrutura existente da mesma forma descrita anteriormente para clusters de membros. A infraestrutura SOFS no cluster de gerenciamento é usada como a referência do namespace do conjunto de clusters (namespace do conjunto de cluster) SOFS. Isso simplesmente significa que cada compartilhamento SMB no namespace do conjunto de clusters SOFS é um compartilhamento de referência – do tipo ' SimpleReferral '-introduzido recentemente no Windows Server 2019.  Essa referência permite que clientes SMB acessem o compartilhamento SMB de destino hospedado no SOFS do cluster de membros. O SOFS de referência do namespace do conjunto de clusters é um mecanismo de referência leve e, dessa forma, não participa do caminho de e/s. As referências de SMB são armazenadas em cache em todos os nós de cliente e o namespace de conjuntos de clusters é atualizado dinamicamente automaticamente essas referências, conforme necessário

## <a name="creating-a-cluster-set"></a>Criando um conjunto de clusters

### <a name="prerequisites"></a>Pré-requisitos

Ao criar um conjunto de clusters, são recomendados os seguintes pré-requisitos:

1. Configure um cliente de gerenciamento que executa o Windows Server 2019.
2. Instale as ferramentas de cluster de failover nesse servidor de gerenciamento.
3. Criar membros do cluster (pelo menos dois clusters com pelo menos dois volumes compartilhados do cluster em cada cluster)
4. Crie um cluster de gerenciamento (físico ou convidado) que se espalha pelos clusters de membros.  Essa abordagem garante que o plano de gerenciamento de conjuntos de clusters continua disponível apesar de possíveis falhas de cluster de membros.

### <a name="steps"></a>Etapas

1. Crie um novo conjunto de clusters de três clusters, conforme definido nos pré-requisitos.  O gráfico abaixo fornece um exemplo de clusters a serem criados.  O nome do conjunto de clusters neste exemplo será **CSMASTER**.

   | Nome do Cluster               | Nome de SOFS de infraestrutura a ser usado posteriormente | 
   |----------------------------|-------------------------------------------|
   | SET-CLUSTER                | SOFS-CLUSTERSET                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. Depois que todos os clusters tiverem sido criados, use os comandos a seguir para criar o mestre de conjunto de clusters.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Para adicionar um servidor de cluster ao conjunto de clusters, o seguinte seria usado.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Se você estiver usando um esquema de endereço IP estático, deverá incluir *-StaticAddress x* . x no comando **New-clusterset** .

4. Depois de criar o conjunto de clusters fora dos membros do cluster, você poderá listar os nós definidos e suas propriedades.  Para enumerar todos os clusters de membros no conjunto de clusters:

        Get-ClusterSetMember -CimSession CSMASTER

5. Para enumerar todos os clusters de membros no conjunto de clusters, incluindo os nós de cluster de gerenciamento:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Para listar todos os nós dos clusters de membros:

        Get-ClusterSetNode -CimSession CSMASTER

7. Para listar todos os grupos de recursos no conjunto de clusters:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Para verificar se o processo de criação do conjunto de clusters criou um compartilhamento SMB (identificado como Volume1 ou qualquer que seja a pasta CSV rotulada com o ScopeName sendo o nome do servidor de arquivos de infraestrutura e o caminho como ambos) na SOFS de infraestrutura para cada membro do cluster Volume CSV:

        Get-SmbShare -CimSession CSMASTER

8. Os conjuntos de clusters têm logs de depuração que podem ser coletados para revisão.  O conjunto de clusters e os logs de depuração do cluster podem ser coletados para todos os membros e o cluster de gerenciamento.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configure a [delegação restrita](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/) de Kerberos entre todos os membros do conjunto de clusters.

10. Configure o tipo de autenticação de migração dinâmica de máquina virtual de cluster cruzado para Kerberos em cada nó no conjunto de clusters.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Adicione o cluster de gerenciamento ao grupo local de administradores em cada nó no conjunto de clusters.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>Criando novas máquinas virtuais e adicionando a conjuntos de clusters

Depois de criar o conjunto de clusters, a próxima etapa é criar novas máquinas virtuais.  Normalmente, quando é hora de criar máquinas virtuais e adicioná-las a um cluster, você precisa fazer algumas verificações nos clusters para ver qual pode ser a melhor execução.  Essas verificações podem incluir:

- Qual a quantidade de memória disponível nos nós de cluster?
- Quanto espaço em disco está disponível nos nós de cluster?
- A máquina virtual requer requisitos de armazenamento específicos (ou seja, quero que o meu SQL Server máquinas virtuais vá para um cluster que executa unidades mais rápidas; ou, a minha máquina virtual de infraestrutura não é tão crítica e pode ser executada em unidades mais lentas).

Depois que essas perguntas forem respondidas, você criará a máquina virtual no cluster de que precisa.  Um dos benefícios dos conjuntos de clusters é que os conjuntos de clusters fazem essas verificações e colocam a máquina virtual no nó ideal.

Os comandos a seguir identificarão o cluster ideal e implantarão a máquina virtual nele.  No exemplo abaixo, uma nova máquina virtual é criada especificando que pelo menos 4 gigabytes de memória estão disponíveis para a máquina virtual e que será necessário utilizar um processador virtual.

- Verifique se há 4 GB disponíveis para a máquina virtual
- definir o processador virtual usado em 1
- Verifique se há pelo menos 10% de CPU disponível para a máquina virtual

   ```PowerShell
   # Identify the optimal node to create a new virtual machine
   $memoryinMB=4096
   $vpcount = 1
   $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10
   $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
   $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

   # Deploy the virtual machine on the optimal node
   Invoke-Command -ComputerName $targetnode.name -scriptblock { param([String]$storagepath); New-VM CSVM1 -MemoryStartupBytes 3072MB -path $storagepath -NewVHDPath CSVM.vhdx -NewVHDSizeBytes 4194304 } -ArgumentList @("\\SOFS-CLUSTER1\VOLUME1") -Credential $cred | Out-Null
   
   Start-VM CSVM1 -ComputerName $targetnode.name | Out-Null
   Get-VM CSVM1 -ComputerName $targetnode.name | fl State, ComputerName
   ```

Quando for concluído, você receberá as informações sobre a máquina virtual e onde ela foi colocada.  No exemplo acima, ele seria mostrado como:

        State         : Running
        ComputerName  : 1-S2D2

Se você não tiver memória suficiente, CPU ou espaço em disco para adicionar a máquina virtual, você receberá o erro:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Depois que a máquina virtual tiver sido criada, ela será exibida no Gerenciador do Hyper-V no nó específico especificado.  Para adicioná-lo como uma máquina virtual de conjunto de clusters e no cluster, o comando está abaixo.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Quando for concluído, a saída será:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Se você tiver adicionado um cluster com máquinas virtuais existentes, as máquinas virtuais também precisarão ser registradas com conjuntos de clusters para registrar todas as máquinas virtuais de uma vez, o comando a ser usado é:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

No entanto, o processo não é concluído, pois o caminho para a máquina virtual precisa ser adicionado ao namespace do conjunto de clusters.

Por exemplo, um cluster existente é adicionado e ele tem máquinas virtuais pré-configuradas que residem no Volume Compartilhado Clusterizado local (CSV), o caminho para o VHDX seria algo semelhante a "C:\ClusterStorage\Volume1\MYVM\Virtual Hard Disks\MYVM.vhdx.  Uma migração de armazenamento precisaria ser realizada, pois os caminhos CSV são por design local para um único cluster de membros. Portanto, o não estará acessível para a máquina virtual depois que ela for migrada ao vivo entre clusters de membros. 

Neste exemplo, CLUSTER3 foi adicionado ao conjunto de clusters usando Add-ClusterSetMember com a infraestrutura Servidor de Arquivos de Escalabilidade Horizontal como SOFS-CLUSTER3.  Para mover a configuração de máquina virtual e o armazenamento, o comando é:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Após a conclusão, você receberá um aviso:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Esse aviso pode ser ignorado, pois o aviso é "nenhuma alteração na configuração de armazenamento da função de máquina virtual foi detectada".  O motivo do aviso como o local físico real não é alterado; somente os caminhos de configuração. 

Para obter mais informações sobre o move-VMStorage, consulte este [link](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

A migração ao vivo de uma máquina virtual entre diferentes clusters de conjuntos de clusters não é a mesma do passado. Em cenários que não são de conjunto de clusters, as etapas seriam:

1. Remova a função de máquina virtual do cluster.
2. Migre ao vivo a máquina virtual para um nó de membro de um cluster diferente.
3. Adicione a máquina virtual ao cluster como uma nova função de máquina virtual.

Com conjuntos de clusters, essas etapas não são necessárias e apenas um comando é necessário.  Primeiro, você deve definir que todas as redes estejam disponíveis para a migração com o comando:

    Set-VMHost -UseAnyNetworkMigration $true

Por exemplo, quero mover uma máquina virtual de conjunto de clusters de CLUSTER1 para NODE2-CL3 no CLUSTER3.  O comando único seria:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Observe que isso não move os arquivos de configuração ou de armazenamento da máquina virtual.  Isso não é necessário, pois o caminho para a máquina virtual permanece como \\SOFS-CLUSTER1\VOLUME1.  Depois que uma máquina virtual é registrada com conjuntos de clusters tem o caminho de compartilhamento do servidor de arquivos de infraestrutura, as unidades e a máquina virtual não precisam estar no mesmo computador que a máquina virtual.

## <a name="creating-availability-sets-fault-domains"></a>Criando domínios de falha de conjuntos de disponibilidade

Conforme descrito na introdução, os domínios de falha do Azure e os conjuntos de disponibilidade podem ser configurados em um conjunto de clusters.  Isso é benéfico para posicionamentos e migrações iniciais de máquinas virtuais entre clusters.  

No exemplo a seguir, há quatro clusters que participam do conjunto de clusters.  Dentro do conjunto, um domínio de falha lógico será criado com dois clusters e um domínio de falha criado com os outros dois clusters.  Esses dois domínios de falha incluirão o conjunto de Availabiilty. 

No exemplo a seguir, CLUSTER1 e CLUSTER2 estarão em um domínio de falha chamado **FD1** enquanto CLUSTER3 e CLUSTER4 estarão em um domínio de falha chamado **FD2**.  O conjunto de disponibilidade será chamado de **CSMASTER** e será composto dos dois domínios de falha.

Para criar os domínios de falha, os comandos são:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Para garantir que eles foram criados com êxito, o Get-ClusterSetFaultDomain pode ser executado com sua saída mostrada.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Agora que os domínios de falha foram criados, o conjunto de disponibilidade precisa ser criado.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Para validar que ele foi criado, use:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Ao criar novas máquinas virtuais, você precisaria usar o parâmetro-Availabilityset como parte da determinação do nó ideal.  Então, ele teria uma aparência semelhante a esta:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Removendo um cluster de conjuntos de clusters devido a vários ciclos de vida. Há ocasiões em que um cluster precisa ser removido de um conjunto de clusters. Como prática recomendada, todas as máquinas virtuais do conjunto de clusters devem ser movidas para fora do cluster. Isso pode ser feito usando os comandos **move-ClusterSetVM** e **move-VMStorage** .

No entanto, se as máquinas virtuais não forem movidas também, os conjuntos de clusters executarão uma série de ações para fornecer um resultado intuitivo ao administrador.  Quando o cluster é removido do conjunto, todas as máquinas virtuais do conjunto de clusters restantes hospedadas no cluster que está sendo removido simplesmente se tornarão máquinas virtuais altamente disponíveis associadas a esse cluster, supondo que eles tenham acesso ao armazenamento.  Os conjuntos de clusters também atualizarão automaticamente seu inventário da:

- Não está mais rastreando a integridade do cluster removido agora e das máquinas virtuais em execução nele
- Remove do namespace do conjunto de clusters e todas as referências a compartilhamentos hospedados no cluster removido agora

Por exemplo, o comando para remover o cluster CLUSTER1 dos conjuntos de clusters seria:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>Perguntas frequentes (FAQ)

**Pergunta:** No meu conjunto de clusters, estou limitado a usar apenas clusters hiperconvergentes? <br>
**Resposta:** Não.  Você pode misturar Espaços de Armazenamento Diretos com clusters tradicionais.

**Pergunta:** Posso gerenciar meu conjunto de clusters por meio de System Center Virtual Machine Manager? <br>
**Resposta:** Atualmente, o System Center Virtual Machine Manager não dá suporte a conjuntos de cluster <br><br> **Pergunta:** Os clusters do Windows Server 2012 R2 ou 2016 podem coexistir no mesmo conjunto de cluster? <br>
**Pergunta:** Posso migrar cargas de trabalho de clusters do Windows Server 2012 R2 ou 2016 simplesmente fazendo com que esses clusters ingressem no mesmo conjunto de cluster? <br>
**Resposta:** Os conjuntos de clusters são uma nova tecnologia introduzida no Windows Server 2019, portanto, não existe em versões anteriores. Clusters de nível inferior baseados em sistema operacional não podem ingressar em um conjunto de clusters. No entanto, a tecnologia de atualizações sem interrupção do sistema operacional do cluster deve fornecer a funcionalidade de migração que você está procurando atualizando esses clusters para o Windows Server 2019.

**Pergunta:** Os conjuntos de clusters podem permitir que eu dimensione o armazenamento ou a computação (sozinho)? <br>
**Resposta:** Sim, simplesmente adicionando um espaço de armazenamento direto ou um cluster do Hyper-V tradicional. Com conjuntos de clusters, é uma alteração simples da taxa de computação para armazenamento, mesmo em um conjunto de clusters hiperconvergente.

**Pergunta:** O que é a ferramenta de gerenciamento para conjuntos de clusters <br>
**Resposta:** PowerShell ou WMI nesta versão.

**Pergunta:** Como a migração dinâmica entre clusters funciona com processadores de diferentes gerações?  <br>
**Resposta:** Os conjuntos de clusters não solucionam as diferenças do processador e substituem o que o Hyper-V suporta atualmente.  Portanto, o modo de compatibilidade do processador deve ser usado com migrações rápidas.  A recomendação para conjuntos de clusters é usar o mesmo hardware de processador em cada cluster individual, bem como todo o conjunto de clusters para migrações ao vivo entre clusters.

**Pergunta:** As máquinas virtuais de conjunto de cluster podem fazer failover automaticamente em uma falha de cluster?  <br>
**Resposta:** Nesta versão, as máquinas virtuais do conjunto de clusters só podem ser migradas ao vivo manualmente entre clusters; Mas não é possível fazer failover automaticamente. 

**Pergunta:** Como garantir que o armazenamento seja resiliente a falhas de cluster? <br>
**Resposta:** Use a solução de réplica de armazenamento de cluster cruzado (SR) em clusters de membros para obter resiliência de armazenamento para falhas de cluster.

**Pergunta:** Uso a réplica de armazenamento (SR) para replicar entre clusters de membros. Os caminhos UNC de armazenamento do namespace do conjunto de clusters mudam no failover do SR para o destino da réplica Espaços de Armazenamento Diretos cluster? <br>
**Resposta:** Nesta versão, uma alteração de referência de namespace de conjunto de clusters não ocorre com o failover do SR. Deixe a Microsoft saber se esse cenário é essencial para você e como você planeja usá-lo.

**Pergunta:** É possível fazer failover de máquinas virtuais entre domínios de falha em uma situação de recuperação de desastre (digamos que o domínio de falha inteiro foi desativado)? <br>
**Resposta:** Não, observe que o failover entre clusters em um domínio de falha lógico ainda não tem suporte. 

**Pergunta:** Meu conjunto de clusters pode abranger clusters em vários sites (ou domínios DNS)? <br> 
**resposta:** este é um cenário não testado e não está imediatamente planejado para o suporte de produção. Deixe a Microsoft saber se esse cenário é essencial para você e como você planeja usá-lo.

**Pergunta:** O conjunto de clusters funciona com IPv6? <br>
**Resposta:** Há suporte para IPv4 e IPv6 com conjuntos de cluster como com clusters de failover.

**Pergunta:** Quais são os requisitos de floresta Active Directory para conjuntos de clusters <br>
**Resposta:** Todos os clusters de membros devem estar na mesma floresta do AD.

**Pergunta:** Quantos clusters ou nós podem fazer parte de um único conjunto de clusters? <br>
**Resposta:** No Windows Server 2019, os conjuntos de cluster foram testados e têm suporte até 64 nós de cluster total. No entanto, a arquitetura de conjuntos de clusters é dimensionada para limites muito maiores e não é algo embutido em código para um limite. Informe a Microsoft se a escala maior é essencial para você e como você planeja usá-la.

**Pergunta:** Todos os clusters de Espaços de Armazenamento Diretos em um conjunto de clusters formam um único pool de armazenamento? <br>
**Resposta:** Não. Espaços de Armazenamento Diretos tecnologia ainda opera em um único cluster e não em clusters de membros em um conjunto de clusters.

**Pergunta:** O namespace do conjunto de clusters está altamente disponível? <br>
**Resposta:** Sim, o namespace do conjunto de clusters é fornecido por meio de um servidor de namespace SOFS de referência (CA) continuamente disponível em execução no cluster de gerenciamento. A Microsoft recomenda ter um número suficiente de máquinas virtuais de clusters de membros para torná-la resiliente a falhas localizadas em todo o cluster. No entanto, para considerar falhas catastróficas inesperadas – por exemplo, todas as máquinas virtuais no cluster de gerenciamento ficam inativas ao mesmo tempo – as informações de referência são armazenadas em cache de forma persistente em cada nó de conjunto de cluster, mesmo entre reinicializações.
 
**Pergunta:** O cluster define o acesso de armazenamento baseado em namespace para reduzir o desempenho do armazenamento em um conjunto de clusters? <br>
**Resposta:** Não. O namespace do conjunto de clusters oferece um namespace de referência de sobreposição dentro de um conjunto de clusters – conceitualmente, como Sistema de Arquivos Distribuído namespaces (DFSN). E, ao contrário de DFSN, todos os metadados de referência de namespace de conjunto de clusters são preenchidos automaticamente e atualizados automaticamente em todos os nós sem nenhuma intervenção do administrador, portanto, quase não há sobrecarga de desempenho no caminho de acesso de armazenamento. 

**Pergunta:** Como fazer backup de metadados do conjunto de clusters? <br>
**Resposta:** Essa diretriz é a mesma do cluster de failover. O backup do estado do sistema também fará o backup do estado do cluster.  Por meio do Backup do Windows Server, você pode fazer restaurações apenas do banco de dados de cluster de um nó (o que nunca deve ser necessário devido a uma série de lógica de auto-recuperação que temos) ou fazer uma restauração autoritativa para reverter todo o banco de dados de cluster em todos os nós. No caso de conjuntos de cluster, a Microsoft recomenda fazer uma restauração autoritativa primeiro no cluster de membros e, em seguida, no cluster de gerenciamento, se necessário.
