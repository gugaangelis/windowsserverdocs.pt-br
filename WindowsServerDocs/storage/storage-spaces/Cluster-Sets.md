---
title: Conjuntos de cluster
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: Este artigo descreve o cenário de conjuntos de Cluster
ms.localizationpriority: medium
ms.openlocfilehash: 2deeb6968f910e80bacb2354ad2e575060a7797a
ms.sourcegitcommit: 07aefbdbb0eedb42aaed3d195c2429310c761da0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/31/2019
ms.locfileid: "9041002"
---
# Conjuntos de cluster

> Aplicável a: Windows Server Insider Preview compilação 17650 e posteriores do

Conjuntos de cluster é a nova tecnologia de escalabilidade horizontal de nuvem nesta versão de visualização que aumenta a contagem de nós de cluster em uma única nuvem Software definido pelo Data Center (SDDC) em ordens de magnitude. Um conjunto de cluster é um agrupamento de flexível de vários Clusters de Failover: de computação, armazenamento ou hiperconvergente. Cluster define tecnologia habilita fluidez de máquina virtual em clusters de membro dentro de um conjunto de cluster e um armazenamento unificado de namespace em um conjunto para dar suporte a fluidez de máquina virtual. 

Enquanto o gerenciamento de Cluster de Failover existente preservando experiências em clusters de membro, uma instância do conjunto de cluster oferece Além disso casos de uso de chave em torno de gerenciamento do ciclo de vida de agregação. Este guia de avaliação do Windows Server Preview cenário fornece as informações necessárias em segundo plano, juntamente com instruções passo a passo para avaliar a tecnologia de conjuntos de cluster usando o PowerShell. 

## Introdução da tecnologia

Tecnologia de conjuntos de cluster é desenvolvida para atender às solicitações de cliente específico operando nuvens Datacenter definido pelo Software (SDDC) em escala. Proposta de valor de conjuntos de cluster nesta versão de visualização pode ser resumida como o seguinte:  

- Aumentar significativamente a escala de nuvem SDDC com suporte para a execução de máquinas virtuais altamente disponíveis combinando vários clusters menores em uma única malha grande, até mesmo, mantendo o limite de falhas de software com um único cluster
- Gerenciar todo o ciclo de vida de Cluster de Failover incluindo integração e desativação clusters, sem afetar a disponibilidade de máquina virtual de locatário, por meio de maneira fluida migrar máquinas virtuais entre essa malha grande
- Alterar a taxa de computação e armazenamento em seu hiperconvergente facilmente I
- Se beneficiar [domínios de falha do Azure semelhantes e disponibilidade define](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) entre clusters no posicionamento inicial da máquina virtual e migração subsequentes máquina virtual
- Mistura-e-correspondência gerações diferentes de hardware de CPU no mesmo cluster definir malha, até mesmo, mantendo os domínios de falha individuais homogêneo para garantir a eficiência máxima.  Observe que a recomendação de hardware mesmo ainda está presente em cada cluster individuais, bem como o conjunto de todo o cluster.

De uma exibição de alto nível, isso é que podem se parecer com conjuntos de cluster.

![Cluster define o modo de exibição de solução](media\Cluster-sets-Overview\Cluster-sets-solution-View.png)

O exemplo a seguir fornece um resumo rápido de cada um dos elementos na imagem acima:

**Cluster de gerenciamento**

Gerenciamento em um conjunto de cluster é um Cluster de Failover que hospeda o plano de gerenciamento altamente disponível do conjunto de todo o cluster e a referência de namespace (Namespace definido Cluster) de armazenamento unificado Scale-Out arquivo SOFS (servidor). Um cluster de gerenciamento é logicamente dissociado de clusters de membro que executam as cargas de trabalho de máquina virtual. Isso torna o cluster que defina o plano de gerenciamento resiliente a qualquer localizadas falhas em todo o cluster, por exemplo, perda de energia de um cluster de membro.   

**Cluster de membro**

Um cluster de membro em um conjunto de cluster normalmente é um cluster de hiperconvergência tradicional executando máquina virtual e cargas de trabalho espaços de armazenamento diretos. Vários clusters de membro participarem de uma implantação de conjunto único cluster, formando a malha de nuvem SDDC maior. Clusters de membro diferem de um cluster de gerenciamento em dois aspectos principais: participarem de clusters de membro no domínio de falha e disponibilidade definida construções e clusters de membro também são dimensionados para a máquina virtual do host e cargas de trabalho espaços de armazenamento diretos. Cluster conjunto de máquinas virtuais que se mover em limites de cluster em um conjunto de cluster não deve ser hospedadas no cluster de gerenciamento por esse motivo.

**Cluster definir referência de namespace SOFS**

Uma referência de namespace do conjunto de cluster (Namespace para definir Cluster) SOFS é um servidor de arquivos de escalabilidade horizontal no qual cada compartilhamento SMB sobre o Cluster definir Namespace SOFS é um compartilhamento de referência – do tipo 'SimpleReferral' recentemente introduzido nesta versão de visualização.  Essa referência permite que os clientes de Server Message Block (SMB) acesso para o destino de compartilhamento SMB hospedado no cluster membro SOFS. Cluster definir referência de namespace SOFS é um mecanismo de referência leve e como tal, não participará no caminho de e/s. As indicações de SMB são armazenadas em cache perpétua na cada um de nós do cliente e o namespace de conjuntos de cluster dinamicamente atualiza automaticamente essas referências conforme necessário.

**Definir mestre de cluster**

Em um conjunto de cluster, a comunicação entre os clusters de membro é flexível e é coordenada por um novo recurso de cluster chamado "Mestre para definir Cluster" (CS-mestre). Como qualquer outro recurso de cluster, CS-mestre é altamente disponível e resistente a falhas de cluster de membro individual e/ou as falhas de nó de cluster de gerenciamento. Por meio de um novo provedor WMI de definir Cluster, CS-mestre fornece o ponto de extremidade de gerenciamento para todas as interações de capacidade de gerenciamento de Cluster definido.

**Trabalho de conjunto de cluster**

Em uma implantação de Cluster definida, o mestre CS interage com um novo recurso de cluster no membro Clusters chamados "Trabalhador para definir Cluster" (CS-trabalhador). CS-trabalhador age como a ligação somente no cluster para coordenar as interações de cluster local conforme solicitado pelo CS-mestre. Exemplos de tais interações incluem posicionamento de máquina virtual e inventário de recurso de cluster local. Há apenas uma instância de trabalhador CS para cada do membro clusters em um conjunto de cluster. 

**Domínio de falha**

Um domínio de falha é o agrupamento de software e artefatos de hardware que determina o administrador podem falhar juntos quando ocorre uma falha.  Enquanto um administrador pode designar um ou mais clusters juntos como um domínio de falha, cada nó pode participar de um domínio de falha em um conjunto de disponibilidade. Cluster define por design deixa a decisão de determinação de limite de domínio de falha para o administrador que está habituado com dados center considerações sobre a topologia – por exemplo, PDU, rede – que compartilham clusters de membro. 

**Conjunto de disponibilidade**

Um conjunto de disponibilidade ajuda o administrador configurar a redundância desejada de cargas de trabalho em cluster em todos os domínios de falha, organizando aqueles em um conjunto de disponibilidade e implantação de cargas de trabalho para esse conjunto de disponibilidade. Digamos que se você estiver implantando um aplicativo de duas camadas, recomendamos que você configure pelo menos duas máquinas virtuais em uma disponibilidade para cada faixa que garante que quando um domínio de falha nesse conjunto de disponibilidade falhar, seu aplicativo pelo menos terá uma máquina virtual em cada camada hospedada em um domínio de falha diferentes desse mesmo conjunto de disponibilidade.

## Por que usar conjuntos de cluster

Conjuntos de cluster fornece a vantagem de escala sem sacrificar resiliência.  

Conjuntos de cluster permite clustering de vários clusters juntos para criar uma malha grande, enquanto cada cluster permanece independente para resiliência.  Por exemplo, você tem um HCI 4 nós vários clusters de máquinas virtuais em execução.  Cada cluster fornece a resiliência necessária para si mesmo.  Se o armazenamento ou a memória é iniciado cheio, escalonamento é a próxima etapa.  Com a expansão vertical, há algumas opções e considerações.

1. Adicione mais armazenamento ao cluster atual.  Com espaços de armazenamento diretos, isso pode ser complicado conforme as mesmas unidades de modelo/firmware exato podem não estar disponíveis.  A consideração de tempos de reconstrução também precisa ser levadas em consideração.
2. Adicione mais memória.  E se você estiver maximizado na memória que podem manipular as máquinas?  E se todos os slots de memória disponível forem completos?
3. Adicione nós de computação adicionais com unidades ao cluster atual.  Isso nos leva volta para a opção 1 que precisam ser considerado.
4. Comprar um novo cluster

Isso é onde os conjuntos de cluster fornece a vantagem de dimensionamento.  Se eu adicionar Meus clusters em um conjunto de cluster, posso podem tirar proveito de memória que pode estar disponível em outro cluster sem qualquer compras adicionais ou armazenamento.  De uma perspectiva de resiliência, adicionar os nós adicionais para um espaços de armazenamento diretos não vai para fornecer votos adicionais para quorum.  Como mencionado [aqui](drive-symmetry-considerations.md), um Cluster de espaços de armazenamento diretos pode sobreviver à perda de 2 nós antes de passar para baixo.  Se você tiver um cluster de 4 nós HCI, 3 nós descem prejudicará todo o cluster.  Se você tiver um cluster de 8 nós, 3 nós descem prejudicará todo o cluster.  Com conjuntos de Cluster que tem dois clusters HCI 4 nós no conjunto, 2 nós em um HCI ir para baixo e 1 nó na outro HCI ir para baixo, dois clusters continua funcionando.  É melhor para criar um cluster de espaços de armazenamento diretos 16 nós grande ou dividi-la em quatro clusters de 4 nós e usar conjuntos de cluster?  Tendo quatro clusters de 4 nós com cluster conjuntos oferece a mesma escala, mas melhor resiliência em que vários nós de computação podem ir para baixo (inesperadamente ou para manutenção) e permanece de produção.

## Considerações sobre a implantação conjuntos de cluster

Ao considerar se conjuntos de cluster é algo que você precisa usar, considere estas perguntas:

- Você precisará ir além dos atual HCI computação e armazenamento escala limites?
- Todos os computação e armazenamento não são idêntica a mesma?
- Você ao vivo migrar máquinas virtuais entre clusters?
- Você gostaria conjuntos de disponibilidade do computador Azure semelhantes e domínios de falha de vários clusters?
- Você precisa levar um tempo para examinar todos os seus clusters para determinar onde qualquer novas máquinas virtuais precisa ser colocada?

Se sua resposta for Sim, cluster conjuntos é que você precisa.

Há alguns outros itens a serem considerados onde um maior SDDC pode alterar suas estratégias de centro de dados geral.  SQL Server é um bom exemplo.  As máquinas virtuais do SQL Server se movendo entre clusters exige SQL para ser executado em nós adicionais de licenciamento?  

## Servidor de arquivos de escalabilidade horizontal e conjuntos de cluster

No Windows Server 2019, há uma nova função de servidor de arquivos de escalabilidade horizontal chamada infraestrutura Scale-Out arquivo SOFS (servidor). 

As considerações a seguir se aplicam a uma função de infraestrutura SOFS:

1.  Pode haver no máximo apenas uma função de cluster de infraestrutura SOFS em um Cluster de Failover. Infraestrutura SOFS função é criada, especificando o "**-infraestrutura**" alternar o parâmetro para o cmdlet **Add-ClusterScaleOutFileServerRole** .  Por exemplo:

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Cada volume CSV criado no failover automaticamente aciona a criação de um compartilhamento SMB com um nome gerado automaticamente com base no nome do volume CSV. Um administrador diretamente não pode criar ou modificar compartilhamentos SMB em uma função SOFS, senão por meio de operações de criar/modificar volume CSV.

3.  Em configurações hiperconvergente, um SOFS infraestrutura permite que um cliente SMB (host do Hyper-V) para se comunicar com garantida contínua disponibilidade (AC) para o servidor de infraestrutura SOFS SMB. Este hiperconvergente SMB loopback CA é obtida por meio de máquinas virtuais acessar seus arquivos de disco virtual (VHDx) onde a identidade proprietária de máquina virtual é encaminhada entre o cliente e servidor. Este encaminhamento de identidade permite que os arquivos de ACL esboços VHDx assim como em configurações de cluster de hiperconvergência padrão como antes.

Depois que um conjunto de cluster é criado, o namespace do conjunto de cluster depende de um SOFS infraestrutura em cada um dos clusters membro e Além disso SOFS uma infraestrutura do cluster de gerenciamento.

No momento em um cluster de membro é adicionado a um conjunto de cluster, o administrador especifica o nome de um SOFS de infraestrutura nesse cluster se já existir. Se o SOFS infraestrutura não existir, uma nova função de infraestrutura SOFS no novo cluster membro é criada, essa operação. Se já existir uma função de infraestrutura SOFS no cluster de membro, a operação de adição implicitamente renomeia para o nome especificado conforme necessário. Servidores SMB singleton existentes, ou funções não - infraestrutura SOFS no membro clusters estão à esquerda as pelo conjunto de cluster. 

No momento em que o conjunto de cluster é criado, o administrador tem a opção de usar um objeto de computador do AD existente como raiz do namespace no cluster de gerenciamento. Operações de criação de conjunto de cluster criar a função de cluster de infraestrutura SOFS no cluster de gerenciamento ou renomeia a função SOFS infraestrutura existente apenas conforme descrita anteriormente para clusters de membro. O SOFS infraestrutura no cluster de gerenciamento é usado como o cluster definir referência de namespace (Namespace para definir Cluster) SOFS. Isso simplesmente significa que cada compartilhamento SMB no cluster de definir o namespace que SOFS é um compartilhamento de referência – do tipo 'SimpleReferral' - recentemente introduzido nesta versão de visualização.  Essa referência permite que os clientes tenham acesso SMB para o destino de compartilhamento SMB hospedados no cluster membro SOFS. Cluster definir referência de namespace SOFS é um mecanismo de referência leve e como tal, não participará no caminho de e/s. As indicações de SMB são armazenadas em cache perpétua na cada um de nós do cliente e o namespace de conjuntos de cluster dinamicamente atualiza automaticamente essas referências conforme necessário

## Criação de um conjunto de Cluster

### Pré-requisitos

Quando criar um cluster de conjunto, você os pré-requisitos a seguir é recomendadas:

1. Configure um cliente de gerenciamento que executam a versão mais recente do Windows Server Insider.
2. Instale as ferramentas de Cluster de Failover neste servidor de gerenciamento.
3. Criar membros do cluster (pelo menos dois clusters pelo menos dois Volumes Compartilhados do Cluster em cada cluster)
4. Crie um cluster de gerenciamento (físico ou convidado) que cobre os clusters de membro.  Essa abordagem garante que os conjuntos de Cluster plano de gerenciamento continua a estar disponível, apesar de falhas de cluster de membro possíveis.

### Etapas

1. Crie um novo cluster definir de três clusters, conforme definido nos pré-requisitos.  O gráfico abaixo fornece um exemplo de clusters de criar.  O nome do cluster definido neste exemplo será **CSMASTER**.

   | Nome do cluster               | Nome de SOFS de infraestrutura para ser usada posteriormente | 
   |----------------------------|-------------------------------------------|
   | SET-CLUSTER                | SOFS CLUSTERSET                           |
   | CLUSTER1                   | SOFS CLUSTER1                             |
   | CLUSTER2                   | SOFS CLUSTER2                             |

2. Depois que todos os cluster foram criados, use os seguintes comandos para criar o cluster conjunto mestre.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Para adicionar um servidor de Cluster para o conjunto de cluster, a seguir seria usado.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Se você estiver usando um esquema de endereço IP estático, você deve incluir *x.x.x. x - StaticAddress* sobre o comando **New-ClusterSet** .

4. Depois de criar o cluster definido fora de membros do cluster, você pode listar o conjunto de nós e suas propriedades.  Enumerar todos os clusters de membro no conjunto de cluster:

        Get-ClusterSetMember -CimSession CSMASTER

5. Enumerar todos os clusters de membro do cluster, incluindo os nós de cluster de gerenciamento:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Para listar todos os nós de clusters de membro:

        Get-ClusterSetNode -CimSession CSMASTER

7. Para listar todos os grupos de recursos em um conjunto de cluster:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Para verificar o cluster definir o processo de criação criado um compartilhamento SMB (identificado como Volume1 ou qualquer pasta CSV é rotulada com o nome_do_escopo sendo o nome do servidor de arquivos de infraestrutura e o caminho como ambos) no SOFS a infraestrutura para cada membro de cluster Volume CSV:

        Get-SmbShare -CimSession CSMASTER

8. Conjuntos de cluster tem logs de depuração que podem ser coletados para análise.  Definir o cluster e logs de depuração do cluster podem ser coletados para todos os membros e o cluster de gerenciamento.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configure Kerberos [delegação restrita](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/) entre todos os membros do conjunto de cluster.

10. Configure o tipo de autenticação de migração dinâmica de máquina virtual de cluster cruzada para Kerberos em cada nó no conjunto de Cluster.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Adicione o cluster de gerenciamento para o grupo de administradores locais em cada nó no conjunto de cluster.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## Criando novas máquinas virtuais e adicionando conjuntos de cluster

Depois de criar o cluster, a próxima etapa é criar novas máquinas virtuais.  Normalmente, quando é hora de criar máquinas virtuais e adicioná-los a um cluster, você precisará fazer algumas verificações nos clusters para ver quais talvez seja melhor executar em.  Essas verificações podem incluir:

- A quantidade de memória está disponível em nós de cluster?
- Quanto espaço em disco está disponível em nós de cluster?
- A máquina virtual exigem requisitos específicos de armazenamento (ou seja, quero minhas máquinas virtuais de SQL Server para ir para um cluster executando unidades mais rápidas; ou, minha máquina virtual de infraestrutura não é tão importante e pode ser executado em unidades mais lentas).

Depois que esse perguntas forem respondidas, você cria a máquina virtual no cluster que precisar ser.  Um dos benefícios dos conjuntos de cluster é que os conjuntos de cluster fazer essas verificações para você e coloque a máquina virtual no nó ideal.

Os comandos abaixo ambos identificará o cluster ideal e implantar a máquina virtual para ele.  No exemplo abaixo, uma nova máquina virtual é criada especificando que pelo menos 4 GB de memória está disponível para a máquina virtual e que ele precisará utilizar 1 processador virtual.

- Certifique-se de que 4gb está disponível para a máquina virtual
- Defina o processador virtual usado em 1
- Verifique se há pelo menos 10% CPU disponível para a máquina virtual

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

Quando for concluída, você terá as informações sobre a máquina virtual e onde ele foi colocado.  No exemplo acima, seria exibido como:

        State         : Running
        ComputerName  : 1-S2D2

Se você não tiver o suficiente memória, cpu ou espaço em disco para adicionar a máquina virtual, você receberá o erro:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Depois que a máquina virtual tiver sido criada, ele será exibido no Gerenciador do Hyper-V no nó específico especificado.  Para adicioná-lo como um cluster de definir a máquina virtual e ao cluster, o comando está abaixo.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Depois de concluída, a saída será:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Se você tiver adicionado um cluster com máquinas virtuais existentes, as máquinas virtuais também precisará ser registrado junto ao Cluster conjuntos Registre todas as máquinas virtuais ao mesmo tempo, usar o comando é:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

No entanto, o processo não for concluído, o caminho para a máquina virtual precisa ser adicionado ao cluster conjunto namespace.

Então, por exemplo, um cluster existente é adicionado e tem máquinas virtuais pré-configuradas o reside no local Volume compartilhado clusterizado (CSV), o caminho para o VHDX seria algo semelhante a "C:\ClusterStorage\Volume1\MYVM\Virtual Disks\MYVM.vhdx disco rígido.  Uma migração do armazenamento precisaria ser feito como CSV caminhos são por design local para um cluster único membro. Assim, não estarão acessíveis para a máquina virtual depois que eles são dinâmicos migrados entre clusters de membro. 

Neste exemplo, CLUSTER3 foi adicionado ao cluster definido usando Add-ClusterSetMember com o servidor de arquivos de escalabilidade horizontal de infraestrutura como CLUSTER3 SOFS.  Para mover a configuração de máquina virtual e o armazenamento, o comando é:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Depois que ele for concluído, você receberá um aviso:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Esse aviso pode ser ignorado como o aviso é "nenhuma alteração na configuração de armazenamento de função de máquina virtual foi detectada".  O motivo para o aviso como a localização física real não muda; somente os caminhos de configuração. 

Para obter mais informações sobre o movimento VMStorage, examine este [link](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

Ao vivo de migração de uma máquina virtual entre clusters de conjunto de cluster diferente é não o mesmo como no passado. Em cenários de conjunto não cluster, as etapas seriam:

1. Remova a função de máquina virtual do Cluster.
2. migração ao vivo a máquina virtual a um nó do membro de um cluster diferente.
3. Adicione a máquina virtual ao cluster como uma nova função de máquina virtual.

Essas etapas não são necessárias com conjuntos de Cluster e apenas um comando é necessário.  Por exemplo, quero mover uma máquina virtual de Cluster definido de CLUSTER1 para CL3 NODE2 em CLUSTER3.  O único comando seria:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Observe que isso não se move os arquivos de armazenamento ou a configuração de máquina virtual.  Isso não é necessário como o caminho para a máquina virtual permanece como \\SOFS-CLUSTER1\VOLUME1.  Depois que uma máquina virtual foi registrada com conjuntos de cluster tem o caminho do compartilhamento de servidor de arquivos de infraestrutura, as unidades e a máquina virtual não exigem sendo no mesmo computador que a máquina virtual.

## Criando disponibilidade conjuntos de domínios de falha

Conforme descrito na introdução, domínios de falha do Azure semelhantes e conjuntos de disponibilidade podem ser configurados em um conjunto de cluster.  Isso é benéfico para migrações entre clusters e posicionamentos inicial da máquina virtual.  

No exemplo a seguir, há quatro clusters participando no conjunto de cluster.  Em conjunto, um domínio de Falha lógica será criado com dois clusters e um domínio de falha criado com os outros dois clusters.  Esses domínios de falha de dois representará o conjunto de Availabiilty. 

No exemplo a seguir, CLUSTER1 e CLUSTER2 serão em um domínio de falha chamado **FD1** enquanto CLUSTER3 e CLUSTER4 estará em um domínio de falha chamado **FD2**.  O conjunto de disponibilidade será chamado **CSMASTER-AS** e ser composto dos domínios de falha de dois.

Para criar os domínios de falha, os comandos são:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Para garantir que eles foram criados com êxito, Get-ClusterSetFaultDomain pode ser executado com sua saída mostrada.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Agora que foram criados os domínios de falha, a disponibilidade definir precisa ser criado.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Para validar que ele foi criado, em seguida, use:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Ao criar novas máquinas virtuais, em seguida, você precisaria usar o parâmetro - AvailabilitySet como parte de determinar o nó ideal.  Portanto, isso seria parecido com isto:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Remover um cluster do cluster define devido a várias ciclos de vida. Há momentos quando um cluster precisa ser removido de um conjunto de cluster. Como prática recomendada, todas as máquinas de virtuais de conjunto de cluster deve ser movidas para fora da cluster. Isso pode ser feito usando os comandos **Mover ClusterSetVM** e **VMStorage de movimento** .

No entanto, se as máquinas virtuais não serão movidas também, conjuntos de cluster executa uma série de ações para fornecer um resultado intuitivo para o administrador.  Quando o cluster é removido do conjunto, todos os demais cluster conjunto de máquinas virtuais hospedadas no cluster que está sendo removido simplesmente tornará máquinas virtuais altamente disponíveis associadas a esse cluster, supondo que eles têm acesso ao seu armazenamento.  Conjuntos de cluster atualizará automaticamente seu inventário por:

- Não há mais de rastreamento a integridade do cluster removido agora e as máquinas virtuais em execução
- Remove do namespace do conjunto de cluster e todas as referências a compartilhamentos hospedados no cluster removido agora

Por exemplo, o comando para remover o cluster CLUSTER1 de conjuntos de cluster seria:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## Perguntas frequentes (FAQ)

**Pergunta:** No meu cluster conjunto, estou limitado ao uso apenas clusters hiperconvergentes? <br>
**Resposta:** Não.  Você pode misturar espaços de armazenamento diretos com clusters tradicionais.

**Pergunta:** Pode gerenciar meu Cluster definido por meio do System Center Virtual Machine Manager? <br>
**Resposta:** System Center Virtual Machine Manager atualmente não oferece suporte a conjuntos de Cluster <br><br> **Pergunta:** Podem Windows Server 2012 R2 ou clusters de 2016 coexistir no mesmo conjunto de cluster? <br>
**Pergunta:** Posso migrar cargas de trabalho desativar o Windows Server 2012 R2 ou 2016 clusters simplesmente tendo esses clusters ingressar o mesmo conjunto de Cluster? <br>
**Resposta:** Conjuntos de cluster é uma nova tecnologia introduzido no Windows Server Preview compilações, portanto, dessa forma, não existe em versões anteriores. Baixo nível do sistema operacional com base em clusters não podem ingressar em um conjunto de cluster. No entanto, a tecnologia de upgrades sem interrupção do sistema operacional de Cluster deve fornecer a funcionalidade de migração que você está procurando por atualizando esses clusters para Windows Server 2019.

**Pergunta:** Conjuntos de Cluster podem permitir que eu dimensionar armazenamento ou de computação (sozinho)? <br>
**Resposta:** Sim, simplesmente adicionando um espaço de armazenamento diretos ou cluster de Hyper-V tradicional. Com conjuntos de cluster, é uma simples alteração de taxa de computação e armazenamento até mesmo em um conjunto de cluster hiperconvergente.

**Pergunta:** O que é que as ferramentas de gerenciamento para conjuntos de cluster <br>
**Resposta:** PowerShell ou WMI nesta versão.

**Pergunta:** Como a migração dinâmica de cluster cruzada funciona com processadores de gerações diferentes?  <br>
**Resposta:** Conjuntos de cluster não contornar diferenças de processador e substituem o que o Hyper-V atualmente oferece suporte.  Portanto, o modo de compatibilidade de processador deve ser usado com migrações rápidas.  A recomendação para conjuntos de Cluster é usar o mesmo hardware de processador dentro de cada Cluster individuais, bem como o conjunto inteiro de Cluster para migrações em tempo real entre clusters ocorrer.

**Pergunta:** Pode meu conjunto de cluster virtual computadores automaticamente em uma falha de cluster de failover?  <br>
**Resposta:** Nesta versão, cluster conjunto de máquinas virtuais só pode ser manualmente migradas ao vivo em clusters; mas não automaticamente failover. 

**Pergunta:** Como podemos garantir armazenamento é resistente a falhas de cluster? <br>
**Resposta:** Use a solução de réplica de armazenamento (SR) entre-cluster em clusters de membro observar a resiliência de armazenamento para falhas de cluster.

**Pergunta:** Posso usar a réplica de armazenamento (SR) para replicar entre clusters de membro. Cluster conjunto namespace armazenamento alteração de caminhos UNC SR failover para o cluster de espaços de armazenamento diretos de destino réplica? <br>
**Resposta:** Nesta versão, tal cluster conjunto namespace indicação alteração não ocorre com SR failover. Informe Microsoft se esse cenário é fundamental para você e como você pretende usá-lo.

**Pergunta:** É possível máquinas virtuais de failover em todos os domínios de falha em uma situação de recuperação de desastres (dizer ao domínio de falha inteiro foi desativado)? <br>
**Resposta:** Não, observe que o failover entre-cluster dentro de uma falha de lógica domínio ainda não tem suporte. 

**Pergunta:** Meu cluster pode definir clusters span em vários sites (ou domínios DNS)? <br> 
**Resposta:** Esse é um cenário não testado e não imediatamente planejada suporte à produção. Informe Microsoft se esse cenário é fundamental para você e como você pretende usá-lo.

**Pergunta:** Cluster definir trabalho com IPv6? <br>
**Resposta:** IPv4 e IPv6 são compatíveis com conjuntos de cluster, assim como acontece com Clusters de Failover.

**Pergunta:** Define quais são os requisitos de floresta do Active Directory para cluster <br>
**Resposta:** Todos os clusters de membro devem estar na mesma floresta do AD.

**Pergunta:** Quantos clusters ou nós pode ser parte de um único cluster definida? <br>
**Resposta:** Na visualização, cluster conjuntos foi testado e suportado até 64 nós de cluster total. No entanto, cluster define escalas de arquitetura para muito maiores limites e não é algo que está codificado para um limite. Informe Microsoft se escala maior é fundamental para você e como você pretende usá-lo.

**Pergunta:** Todos os clusters de espaços de armazenamento diretos em um conjunto de cluster serão formam um pool de armazenamento único? <br>
**Resposta:** Não. Tecnologia de armazenamento em espaços diretos ainda opera em um único cluster e não entre clusters de membro em um conjunto de cluster.

**Pergunta:** O cluster é definido namespace altamente disponível? <br>
**Resposta:** Sim, o namespace do conjunto de cluster é fornecido por meio de um servidor de namespace continuamente disponíveis (AC) referência SOFS em execução no cluster de gerenciamento. A Microsoft recomenda suficiente número de máquinas virtuais de clusters de membro para torná-lo resistente a falhas de todo o cluster localizadas. No entanto, para levar em conta falhas inesperadas catastróficas – por exemplo, todas as máquinas virtuais no cluster de gerenciamento indo para baixo, ao mesmo tempo – as informações de referência é Além disso constantemente armazenado em cache em cada nó do cluster conjunto, até mesmo entre as reinicializações.
 
**Pergunta:** O cluster define o acesso de armazenamento baseado no namespace lento para baixo desempenho de armazenamento em um conjunto de cluster? <br>
**Resposta:** Não. Namespace do conjunto de cluster oferece um namespace de indicação de sobreposição dentro de um conjunto de cluster – conceitualmente como distribuídos arquivo sistema Namespaces (DFSN). E, ao contrário de DFSN, todos os metadados de referência do namespace conjunto de cluster são preenchido automaticamente e atualizado automaticamente em todos os nós sem nenhuma intervenção do administrador, portanto, não é quase sem sobrecarga de desempenho no caminho de acesso de armazenamento. 

**Pergunta:** Como posso fazer backup de metadados do conjunto de cluster? <br>
**Resposta:** Essa orientação é o mesmo de Cluster de Failover. O Backup de estado do sistema fará o backup o estado do cluster.  Por meio do Backup do Windows Server, você pode fazer restaurações de apenas de um nó cluster banco de dados (que nunca deve ser necessário devido a um pouco de lógica auto-recuperação temos) ou fazer uma restauração autoritativa para reverter o banco de dados do cluster inteiro em todos os nós. No caso de conjuntos de cluster, a Microsoft recomenda fazendo tal uma restauração autoritativa primeiro no cluster membro e, em seguida, o cluster de gerenciamento, se necessário. 
