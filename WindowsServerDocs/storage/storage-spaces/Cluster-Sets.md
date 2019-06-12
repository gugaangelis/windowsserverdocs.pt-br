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
ms.openlocfilehash: 349b69835ae68c626e886cad30f4d5a89d358372
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719710"
---
# <a name="cluster-sets"></a>Conjuntos de cluster

> Aplica-se a: Windows Server 2019

Conjuntos de cluster é a nova tecnologia de expansão de nuvem na versão do Windows Server 2019 que aumenta a contagem de nós de cluster em uma única nuvem de Software definida pelo Data Center SDDC () em ordens de magnitude. Um conjunto de cluster é um agrupamento flexível de vários Clusters de Failover: computação, armazenamento ou hiperconvergente. Cluster conjuntos de tecnologia habilita fluidez da máquina virtual em clusters de membro dentro de um conjunto de cluster e um namespace unificado de armazenamento em todo o conjunto para dar suporte a fluidez de máquina virtual.

Enquanto o gerenciamento de Cluster de Failover existente preservando experiências em clusters de membro, uma instância de cluster de conjunto Além disso oferece principais casos de uso em torno do gerenciamento de ciclo de vida na agregação. Este guia de avaliação do Windows Server 2019 cenário fornece as informações necessárias em segundo plano, juntamente com instruções passo a passo para avaliar a tecnologia de conjuntos de cluster usando o PowerShell.

## <a name="technology-introduction"></a>Introdução da tecnologia

Tecnologia de conjuntos de cluster é desenvolvida para atender às solicitações de cliente específico operar nuvens de Software definidas SDDC (Datacenter) em escala. Proposta de valor de conjuntos de cluster pode ser resumida da seguinte maneira:  

- Aumentar significativamente a escala de nuvem SDDC com suporte para a execução de máquinas virtuais altamente disponíveis, combinando vários clusters menores em uma única malha grande, até mesmo enquanto mantém o limite de falha de software em um único cluster
- Gerenciar todo ciclo de vida do Cluster de Failover incluindo integração e desativação de clusters, sem afetar a disponibilidade de máquina virtual de locatário, por meio de fluidez migrando máquinas virtuais entre esta malha grande
- Alterar facilmente a taxa de computação e armazenamento no seu hiperconvergente eu
- Se beneficiar [do Azure como domínios de falha e conjuntos de disponibilidade](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) entre os clusters no posicionamento de máquina virtual inicial e a migração de máquina de virtual subsequentes
- Misturar e combinar diferentes gerações de hardware de CPU no mesmo cluster definido malha, até mesmo, mantendo os domínios de falha individuais homogêneos para máxima eficiência.  Observe que a recomendação do mesmo hardware ainda está presente em cada cluster individual, bem como o conjunto de todo o cluster.

De uma exibição de alto nível, isso é qual cluster podem se parecer com conjuntos.

![Cluster define o modo de exibição de solução](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

O exemplo a seguir fornece um resumo de cada um dos elementos na imagem acima:

**Cluster de gerenciamento**

Gerenciamento em um conjunto de cluster é um Cluster de Failover que hospeda o plano de gerenciamento altamente disponível do conjunto de todo o cluster e a referência de namespace (Namespace definido do Cluster) de armazenamento unificado servidor de arquivos de escalabilidade horizontal (SOFS). Um cluster de gerenciamento é logicamente separado de clusters de membro que executam as cargas de trabalho de máquina virtual. Isso torna o cluster definir plano de gerenciamento flexível para quaisquer falhas localizadas de todo o cluster, por exemplo, perda de energia de um cluster de membro.   

**Cluster de membro**

Um cluster de membro em um conjunto de cluster normalmente é um cluster hiperconvergente tradicional que executam cargas de trabalho de espaços de armazenamento diretos e de máquina virtual. Participarem de vários clusters de membro em uma implantação de conjunto único cluster, que formam a malha de nuvem SDDC maior. Clusters de membro são diferentes de um cluster de gerenciamento em dois aspectos-chave: clusters de membro participarem no domínio de falha e construções de conjunto de disponibilidade e clusters de membro também são dimensionados para a máquina virtual do host e cargas de trabalho de espaços de armazenamento diretos. Cluster conjunto de máquinas virtuais que se movem entre os limites de cluster em um conjunto de cluster não deve ser hospedadas no cluster de gerenciamento por esse motivo.

**Cluster definir referência de namespace SOFS**

Uma referência de namespace do conjunto de cluster (Namespace para definir Cluster) SOFS é um servidor de arquivos de escalabilidade horizontal no qual cada compartilhamento SMB no Cluster defina Namespace SOFS é um compartilhamento de referência – do tipo 'SimpleReferral' recentemente introduzido no Windows Server 2019. Esta referência permite que os clientes de bloco de mensagens de servidor (SMB) para acesso ao destino de compartilhamento SMB hospedado no cluster SOFS membro. O cluster definir referência de namespace SOFS é um mecanismo leve de referência e como tal, não participar no caminho de e/s. As referências de SMB são armazenados em cache perpetuamente em cada um de nós de cliente e o namespace de conjuntos do cluster dinamicamente atualiza automaticamente essas referências conforme necessário.

**Define o cluster mestre**

Em um conjunto de cluster, a comunicação entre os clusters de membro está acoplada livremente e é coordenada por um novo recurso de cluster chamado "Mestre para definir Cluster" (CS-mestre). Como qualquer outro recurso de cluster, CS-mestre está altamente disponível e resistente a falhas de cluster de membro individual e/ou as falhas de nó de cluster de gerenciamento. Por meio de um novo provedor de WMI do Cluster definida, o CS-mestre fornece o ponto de extremidade de gerenciamento para todas as interações de capacidade de gerenciamento de Cluster definido.

**Operador de conjunto de cluster**

Em uma implantação de Cluster definida, o CS-mestre interage com um novo recurso de cluster no membro Clusters chamados de "Trabalho para definir Cluster" (CS-Worker). CS-Worker atua como o elo somente no cluster para orquestrar as interações de cluster local, conforme solicitado pelo mestre de CS. Exemplos de tais interações incluem o posicionamento da máquina virtual e o inventário de recursos de cluster local. Há apenas uma instância de CS-Worker para cada um dos membro clusters em um conjunto de cluster. 

**domínio de falha**

Um domínio de falha é o agrupamento de software e artefatos de hardware que o administrador determina poderia falhar juntos quando ocorre uma falha.  Embora um administrador pode designar um ou mais clusters juntos como um domínio de falha, cada nó pode participar de um domínio de falha em um conjunto de disponibilidade. Cluster define pelo design deixa a decisão de determinação de limites de domínio de falha para o administrador que está bem familiarizado com considerações de topologia de centro de dados – por exemplo, PDU, de rede – que compartilhem de clusters de membro. 

**Conjunto de disponibilidade**

Um conjunto de disponibilidade ajuda o administrador configurar a redundância desejada das cargas de trabalho clusterizadas entre domínios de falha, organizando-los em um conjunto de disponibilidade e implantando cargas de trabalho no conjunto de disponibilidade. Digamos que, se você estiver implantando um aplicativo de duas camadas, é recomendável que você configure pelo menos duas máquinas virtuais no conjunto de disponibilidade de cada camada que garantirá que, quando um domínio de falha no conjunto de disponibilidade é desativado, seu aplicativo será tenha pelo menos uma máquina virtual em cada camada hospedada em um domínio de falha diferentes do mesmo conjunto de disponibilidade.

## <a name="why-use-cluster-sets"></a>Por que usar conjuntos de cluster

Conjuntos de cluster fornece o benefício de escala sem sacrificar a resiliência.  

Conjuntos de cluster permite para clustering de vários clusters em conjunto para criar uma malha de grande, enquanto cada cluster permanece independente para garantir a resiliência.  Por exemplo, você tem um HCI de 4 nós vários clusters de máquinas virtuais em execução.  Cada cluster fornece a resiliência necessária para si mesmo.  Se a memória ou armazenamento começa a ficar cheio, escalar verticalmente será a próxima etapa.  Com o dimensionamento para cima, há algumas opções e considerações.

1. Adicione mais armazenamento ao cluster atual.  Com espaços de armazenamento diretos, este pode ser complicado pois as mesmas unidades de modelo/firmware exata podem não estar disponíveis.  A consideração de tempos de reconstrução também precisam ser levadas em conta.
2. Adicione mais memória.  E se você é maximizadas na memória podem lidar com os computadores?  E se todos os slots de memória disponível são completos?
3. Adicione nós de computação adicionais com as unidades para o cluster atual.  Isso nos leva ao passado à opção 1 que precisam ser considerados.
4. Um cluster totalmente novo de compra

Isso é onde os conjuntos de cluster fornece o benefício de dimensionamento.  Se eu adicionar meu cluster em um conjunto de cluster, posso aproveitar de armazenamento ou de memória que pode estar disponível em outro cluster sem compras adicionais.  De uma perspectiva de resiliência, adicionar os nós adicionais a um armazenamento de espaços diretos não vai fornecer adicionais votos de quorum.  Conforme mencionado [aqui](drive-symmetry-considerations.md), um Cluster de espaços de armazenamento diretos pode sobreviver à perda de 2 nós antes de ir para baixo.  Se você tiver um cluster de 4 nós HCI, 3 nós descem desativarão todo o cluster.  Se você tiver um cluster de 8 nós, 3 nós descem desativarão todo o cluster.  Com conjuntos de Cluster que tem dois clusters HCI de 4 nós no conjunto, 2 nós em um HCI descem e 1 nó em que os outro HCI ficar inativo, ambos os clusters continua funcionando.  É melhor criar um cluster de espaços de armazenamento diretos de 16 nós grande ou dividi-la em quatro clusters de 4 nós e usar conjuntos de cluster?  Ter quatro clusters de 4 nós com cluster conjuntos oferece a mesma escala, mas a melhor resiliência em que vários nós de computação podem ficar inativo (inesperadamente ou para manutenção) e falta de produção.

## <a name="considerations-for-deploying-cluster-sets"></a>Considerações sobre a implantação de conjuntos de cluster

Ao considerar se os conjuntos de cluster é algo que você precisa usar, considere estas perguntas:

- Você precisa ir além do atual HCI computação e armazenamento limites de escala?
- Computação e o armazenamento não são identicamente iguais?
- Em tempo real você migrar máquinas virtuais entre clusters?
- Você deseja domínios de falha e conjuntos de disponibilidade do computador Azure semelhante entre vários clusters?
- Você precisa dedicar algum tempo para examinar todos os seus clusters para determinar em que qualquer nova máquina virtual precisa ser colocado?

Se sua resposta for Sim, os conjuntos de cluster é o que você precisa.

Há alguns outros itens a serem considerados em que uma maior SDDC pode mudar suas estratégias de centro de dados geral.  SQL Server é um bom exemplo.  Movendo máquinas de virtuais do SQL Server entre clusters requer licenciamento do SQL para execução em nós adicionais?  

## <a name="scale-out-file-server-and-cluster-sets"></a>Servidor de arquivos de expansão e conjuntos de cluster

No Windows Server de 2019, há uma nova função de servidor de arquivos escalável chamada infraestrutura escalável arquivo SOFS (servidor). 

As seguintes considerações se aplicam a uma função de infraestrutura SOFS:

1.  Pode haver no máximo apenas uma função de cluster de SOFS de infraestrutura em um Cluster de Failover. Função SOFS de infraestrutura é criada especificando o " **-infra-estrutura**" Troque o parâmetro para o **ClusterScaleOutFileServerRole adicionar** cmdlet.  Por exemplo:

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Cada volume CSV criada no failover automaticamente dispara a criação de um compartilhamento SMB com um nome gerado automaticamente com base no nome do volume CSV. Um administrador não pode criar ou modificar compartilhamentos SMB em uma função de SOFS diferente por meio de operações criar/modificar de volumes CSV diretamente.

3.  Em configurações hiperconvergentes, um SOFS de infra-estrutura, permite que um cliente SMB (host Hyper-V) para se comunicar com garantia disponibilidade contínua (CA) com o servidor SMB de SOFS de infraestrutura. Este hiperconvergente SMB loopback da autoridade de certificação é obtida por meio de máquinas virtuais que acessam seus arquivos de disco virtual (VHDx) em que a identidade da máquina virtual proprietário é encaminhada entre o cliente e servidor. Esse encaminhamento de identidade permite que arquivos VHDx de aplicação de ACL em assim como em configurações de cluster hiperconvergente padrão como antes.

Depois de criar um conjunto de cluster, o namespace do conjunto de cluster se baseia em um SOFS de infraestrutura em cada um dos membro clusters e, além disso, um SOFS de infraestrutura no cluster de gerenciamento.

No momento em um cluster de membro é adicionado a um conjunto de cluster, o administrador especifica o nome do SOFS infra-estrutura nesse cluster se já existir um. Se o SOFS de infraestrutura não existir, uma nova função de SOFS de infraestrutura no cluster novo membro é criada por esta operação. Se já existir uma função de SOFS de infraestrutura no cluster de membro, a operação de adição implicitamente renomeia o arquivo como o nome especificado conforme necessário. Quaisquer servidores SMB de singleton existentes, ou funções não - infraestrutura SOFS no membro clusters são deixados não utilizada pelo conjunto de cluster. 

No momento em que o conjunto de cluster é criado, o administrador tem a opção de usar um objeto de computador do AD já existente como a raiz do namespace no cluster de gerenciamento. Operações de criação do cluster conjunto criar a função de cluster de SOFS de infraestrutura no cluster de gerenciamento ou renomeia a função de infraestrutura SOFS existente apenas conforme descrita anteriormente para clusters de membro. O SOFS de infraestrutura no cluster de gerenciamento é usado como o cluster do conjunto de referência de namespace (Namespace para definir Cluster) SOFS. Isso simplesmente significa que cada compartilhamento SMB no cluster defina namespace que SOFS é um compartilhamento de referência – do tipo 'SimpleReferral' - recentemente introduzido no Windows Server 2019.  Esta referência permite acesso de clientes SMB para o destino de compartilhamento SMB hospedado no cluster SOFS membro. O cluster definir referência de namespace SOFS é um mecanismo leve de referência e como tal, não participar no caminho de e/s. As referências de SMB são armazenados em cache perpetuamente em cada um de nós de cliente e o namespace de conjuntos do cluster dinamicamente atualiza automaticamente essas referências conforme necessário

## <a name="creating-a-cluster-set"></a>Criação de um conjunto de Cluster

### <a name="prerequisites"></a>Pré-requisitos

Ao criar um cluster de conjunto, você os pré-requisitos a seguir é recomendadas:

1. Configure um cliente de gerenciamento executando o Windows Server 2019.
2. Instale as ferramentas de Cluster de Failover no servidor de gerenciamento.
3. Criar membros do cluster (pelo menos dois clusters pelo menos dois Volumes Compartilhados do Cluster em cada cluster)
4. Crie um cluster de gerenciamento (físico ou convidado) que amplia os clusters de membro.  Essa abordagem garante que os conjuntos de Cluster no plano de gerenciamento continua disponível, independentemente de falhas de cluster de membro possíveis.

### <a name="steps"></a>Etapas

1. Crie um novo cluster de conjunto de três clusters conforme definido nos pré-requisitos.  O gráfico abaixo fornece um exemplo de clusters para criar.  O nome do cluster definido neste exemplo será **CSMASTER**.

   | Nome do Cluster               | Nome do SOFS infraestrutura a ser usado mais tarde | 
   |----------------------------|-------------------------------------------|
   | SET-CLUSTER                | SOFS-CLUSTERSET                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. Depois que todos os cluster tiverem sido criados, use os seguintes comandos para criar o cluster conjunto mestre.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Para adicionar um servidor de Cluster para o conjunto de cluster, a seguir seria usado.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Se você estiver usando um esquema de endereço IP estático, você deve incluir *x.x.x. x de - StaticAddress* sobre o **New ClusterSet** comando.

4. Depois de criar o cluster definido fora de membros do cluster, você pode listar o conjunto de nós e suas propriedades.  Para enumerar todos os clusters do membro no conjunto de cluster:

        Get-ClusterSetMember -CimSession CSMASTER

5. Para enumerar todos os clusters de membro do cluster, incluindo os nós de cluster de gerenciamento:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Para listar todos os nós de clusters de membro:

        Get-ClusterSetNode -CimSession CSMASTER

7. Para listar todos os grupos de recursos em todo o conjunto de cluster:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Para verificar se o cluster definir o processo de criação criado um compartilhamento SMB (identificado como Volume1 ou qualquer pasta CSV é rotulada com o ScopeName sendo o nome do servidor de arquivos de infraestrutura e o caminho como "ambas") no SOFS infraestrutura para cada membro do cluster Volume CSV:

        Get-SmbShare -CimSession CSMASTER

8. Conjuntos de cluster tem logs de depuração que podem ser coletados para análise.  Definir o cluster e os logs de depuração de cluster podem ser coletados para todos os membros e o grupo de gerenciamento.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configurar o Kerberos [delegação restrita](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/) entre o cluster de todos os membros do conjunto.

10. Configure o tipo de autenticação de migração ao vivo de máquina de virtual entre clusters para Kerberos em cada nó no Cluster definido.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Adicione o grupo de gerenciamento para o grupo de administradores locais em cada nó no conjunto de cluster.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>Criando novas máquinas virtuais e adicionando a conjuntos de cluster

Depois de criar o cluster, a próxima etapa é criar novas máquinas virtuais.  Normalmente, quando é hora de criar máquinas virtuais e adicioná-los a um cluster, você precisa fazer algumas verificações nos clusters para ver quais talvez seja melhor executar em.  Essas verificações podem incluir:

- A quantidade de memória está disponível em nós de cluster?
- Quanto espaço em disco está disponível em nós de cluster?
- A máquina virtual exige requisitos de armazenamento específico (ou seja, quero que meu máquinas de virtuais do SQL Server para ir para um cluster executando unidades mais rápidas; ou, minha máquina virtual de infraestrutura não é tão importante e pode ser executado em unidades mais lentas).

Após essa perguntas são respondidas, você cria a máquina virtual no cluster que você precise que seja.  Um dos benefícios dos conjuntos de cluster é que o cluster conjuntos fazem essas verificações para você e colocar a máquina virtual no nó ideal.

Os comandos a seguir ambos identificará o cluster ideal e implante a máquina virtual a ele.  No exemplo abaixo, uma nova máquina virtual é criada, especificando que pelo menos 4 GB de memória está disponível para a máquina virtual e que ele precisará utilizar a 1 processador virtual.

- Certifique-se de que 4gb está disponível para a máquina virtual
- definir o processador virtual usado em 1
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

Quando ele for concluído, você terá as informações sobre a máquina virtual e onde ele foi colocado.  No exemplo acima, seria exibido como:

        State         : Running
        ComputerName  : 1-S2D2

Se você não tem suficiente memória, cpu ou espaço em disco para adicionar a máquina virtual, você receberá o erro:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Quando a máquina virtual tiver sido criada, ele será exibido no Gerenciador do Hyper-V no nó específico especificado.  Para adicioná-lo como um cluster do conjunto de máquina virtual e o cluster, o comando está abaixo.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Quando ele for concluído, a saída será:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Se você tiver adicionado um cluster com máquinas virtuais existentes, as máquinas virtuais também precisará ser registrado com o Cluster conjuntos então registrar todas as máquinas virtuais ao mesmo tempo, o comando a ser usado é:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

No entanto, o processo não é completo, como o caminho para a máquina virtual precisa ser adicionado ao namespace do conjunto de cluster.

Por exemplo, um cluster existente é adicionado e ele tem máquinas virtuais pré-configuradas a residem no local Cluster CSV (Volume compartilhado), o caminho para o VHDX seria algo semelhante a "C:\ClusterStorage\Volume1\MYVM\Virtual rígido Disks\MYVM.vhdx.  Uma migração de armazenamento precisa ser feito como caminhos CSV são propositalmente local para um cluster único membro. Portanto, não estará acessível para a máquina virtual depois que eles ocorrem em tempo real migrados entre clusters de membro. 

Neste exemplo, CLUSTER3 foi adicionado ao cluster definido usando Add-ClusterSetMember com o servidor de arquivos de escalabilidade horizontal de infra-estrutura como CLUSTER3 SOFS.  Para mover a configuração da máquina virtual e o armazenamento, o comando é:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Quando for concluído, você receberá um aviso:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Esse aviso pode ser ignorado, pois o aviso é "foram detectadas sem alterações na configuração de armazenamento da função de máquina virtual".  O motivo para o aviso como o local físico real não é alterado; somente os caminhos de configuração. 

Para obter mais informações sobre o Move-VMStorage, examine este [link](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

Ao vivo de migração de uma máquina virtual entre clusters de conjunto de cluster diferente não é o mesmo como no passado. Em cenários de conjunto não clusterizado, as etapas seriam:

1. Remova a função de máquina virtual do Cluster.
2. migração ao vivo a máquina virtual para um nó de membro de um cluster diferente.
3. Adicione a máquina virtual no cluster como uma nova função de máquina virtual.

Com conjuntos de Cluster, essas etapas não são necessárias e apenas um comando é necessária.  Primeiro, você deve definir todas as redes estejam disponíveis para a migração com o comando:

    Set-VMHost -UseAnyNetworkMigration $true

Por exemplo, eu quero mover uma máquina virtual de Cluster definido de CLUSTER1 para NODE2 CL3 em CLUSTER3.  O único comando seria:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Observe que isso não move os arquivos de armazenamento ou a configuração de máquina virtual.  Isso não é necessário, pois o caminho para a máquina virtual permanece como \\CLUSTER1\VOLUME1 SOFS.  Depois que uma máquina virtual tiver sido registrada com conjuntos de cluster tem o caminho de compartilhamento do servidor de arquivos de infraestrutura, as unidades e a máquina virtual não exigem que está sendo no mesmo computador que a máquina virtual.

## <a name="creating-availability-sets-fault-domains"></a>Criar conjuntos de disponibilidade domínios de falha

Conforme descrito na introdução, domínios de falha como do Azure e conjuntos de disponibilidade podem ser configurados em um conjunto de cluster.  Isso é benéfico para os posicionamentos de máquina virtual inicial e migrações entre clusters.  

No exemplo a seguir, há quatro clusters que participam no conjunto de cluster.  Dentro do conjunto de um domínio de Falha lógica será criado com dois clusters e um domínio de falha criado com os outros dois clusters.  Esses dois domínios de falha serão compõem o conjunto de disponibilidade. 

No exemplo a seguir, CLUSTER1 e o CLUSTER2 estarão em um domínio de falha chamado **FD1** enquanto CLUSTER3 e CLUSTER4 estarão em um domínio de falha chamado **FD2**.  O conjunto de disponibilidade será chamado **CSMASTER-AS** e ser composto de dois domínios de falha.

Para criar os domínios de falha, os comandos são:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Para garantir que eles foram criados com êxito, o Get-ClusterSetFaultDomain pode ser executado com a saída mostrada.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Agora que criou os domínios de falha, o conjunto de disponibilidade precisa ser criado.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Para validar que ele tiver sido criado, em seguida, use:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Ao criar novas máquinas virtuais, em seguida, você precisaria usar o parâmetro - AvailabilitySet como parte de determinar o nó ideal.  Portanto, ele teria aparência semelhante a esta:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Removendo um cluster do cluster define devido a vários ciclos de vida. Há vezes quando um cluster precisa ser removido de um conjunto de cluster. Como prática recomendada, todas as máquinas de virtuais do conjunto de cluster devem ser movidas fora do cluster. Isso pode ser feito usando o **movimentação ClusterSetVM** e **Move-VMStorage** comandos.

No entanto, se as máquinas virtuais não serão movidas também, conjuntos de cluster executa uma série de ações para fornecer um resultado intuitivo para o administrador.  Quando o cluster é removido do conjunto, todos os restantes cluster conjunto de máquinas virtuais hospedadas no cluster que está sendo removido simplesmente tornará máquinas virtuais altamente disponíveis, associadas a esse cluster, supondo que eles têm acesso ao seu armazenamento.  Conjuntos de cluster atualizará automaticamente o seu inventário por:

- Não há mais acompanhamento da integridade do cluster agora-removido e as máquinas virtuais em execução
- Remove do namespace do conjunto de cluster e todas as referências a compartilhamentos hospedadas no cluster removido de agora

Por exemplo, o comando para remover o cluster CLUSTER1 de conjuntos de cluster seria:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>Perguntas frequentes (FAQ)

**Pergunta:** Em meu conjunto de cluster, estou limitado a usar somente os clusters hiperconvergente? <br>
**Resposta:** Não.  Você pode combinar espaços de armazenamento diretos com clusters tradicionais.

**Pergunta:** Pode gerenciar meu Cluster definido por meio do System Center Virtual Machine Manager? <br>
**Resposta:** System Center Virtual Machine Manager não oferece suporte a conjuntos de Cluster <br><br> **Pergunta:** Podem Windows Server 2012 R2 ou 2016 clusters coexistir no mesmo conjunto de cluster? <br>
**Pergunta:** Posso migrar cargas de trabalho do Windows Server 2012 R2 ou 2016 clusters por simplesmente ter esses clusters ingressar o mesmo conjunto de Cluster? <br>
**Resposta:** Conjuntos de cluster é uma nova tecnologia que estão sendo introduzida no Windows Server de 2019, então, como tal, não existe nas versões anteriores. Clusters com base no sistema operacional de nível inferior não podem ingressar em um conjunto de cluster. No entanto, a tecnologia de atualizações sem interrupção do sistema operacional do Cluster deve fornecer a funcionalidade de migração que você está procurando atualizando esses clusters para o Windows Server 2019.

**Pergunta:** Podem conjuntos de Cluster permite dimensionar o armazenamento ou de computação (sozinho)? <br>
**Resposta:** Sim, simplesmente adicionando um espaço de armazenamento diretos ou cluster do Hyper-V tradicional. Com conjuntos de cluster, é uma simples alteração de taxa de computação e armazenamento, mesmo em um conjunto de cluster hiperconvergente.

**Pergunta:** O que é a ferramenta de gerenciamento para conjuntos de cluster <br>
**Resposta:** PowerShell ou WMI nesta versão.

**Pergunta:** Como a migração dinâmica entre clusters funcionará com processadores de gerações diferentes?  <br>
**Resposta:** Conjuntos de cluster não alternativas para diferenças de processador e substituem o Hyper-V atualmente suporta.  Portanto, o modo de compatibilidade do processador deve ser usado com migrações rápidas.  A recomendação para conjuntos de Cluster é usar o mesmo hardware de processador em cada Cluster individual, bem como o Cluster inteiro definido para migrações ao vivo entre clusters ocorra.

**Pergunta:** Pode meu conjunto de cluster virtual automaticamente máquinas failover em um caso de falha do cluster?  <br>
**Resposta:** Nesta versão, conjunto de máquinas de virtuais do cluster só pode ser manualmente migradas ao vivo em clusters; mas não é automaticamente failover. 

**Pergunta:** Como podemos garantir o armazenamento é resistente a falhas de cluster? <br>
**Resposta:** Use solução de réplica de armazenamento (SR) entre clusters em clusters de membro perceber a resiliência de armazenamento para falhas de cluster.

**Pergunta:** Posso usar SR (réplica armazenamento) para replicar entre clusters de membro. Cluster armazenamento de namespace do conjunto de alteração de caminhos UNC SR failover para o cluster de espaços de armazenamento diretos de destino de réplica? <br>
**Resposta:** Nesta versão, uma alteração desse tipo cluster conjunto namespace referência não ocorre com o failover do SR. Informe à Microsoft saber se esse cenário é crítico para você e como você planeja usá-lo.

**Pergunta:** É possível executar failover em máquinas virtuais entre domínios de falha em uma situação de recuperação de desastres (digamos, o domínio de falha inteiro foi desativado)? <br>
**Resposta:** Não, observe que o failover entre clusters dentro de uma Falha lógica domínio ainda não é suportado. 

**Pergunta:** Meu cluster definir clusters alcance em vários sites (ou domínios DNS)? <br> 
**Resposta:** Isso é um cenário não testado e não imediatamente planejados para suporte de produção. Informe à Microsoft saber se esse cenário é crítico para você e como você planeja usá-lo.

**Pergunta:** Cluster de definir o trabalho com o IPv6? <br>
**Resposta:** Assim como acontece com os Clusters de Failover, o IPv4 e IPv6 são compatíveis com os conjuntos de cluster.

**Pergunta:** Define quais são os requisitos de florestas do Active Directory para o cluster <br>
**Resposta:** Todos os clusters de membro devem estar na mesma floresta do AD.

**Pergunta:** O número de nós ou clusters pode ser parte de um único cluster definida? <br>
**Resposta:** No Windows Server de 2019, conjuntos de cluster foram testados e suporte para até 64 nós total do cluster. No entanto, cluster define as escalas de arquitetura para limites muito maiores e não é algo que é codificado para um limite. Informe à Microsoft saber se a escala maior é crítica para você e como você planeja usá-lo.

**Pergunta:** Todos os clusters de espaços de armazenamento diretos em um conjunto de cluster formarão um único pool de armazenamento? <br>
**Resposta:** Não. Tecnologia de armazenamento de espaços diretos ainda opera em um único cluster e não entre clusters de membro em um conjunto de cluster.

**Pergunta:** O cluster está definido namespace altamente disponível? <br>
**Resposta:** Sim, o namespace do conjunto de cluster é fornecido por meio de um servidor de namespace continuamente disponíveis (CA) referência SOFS em execução no cluster de gerenciamento. A Microsoft recomenda ter um número suficiente de máquinas virtuais de clusters de membro para tornar resilientes a falhas localizadas de todo o cluster. No entanto, para levar em conta falhas catastróficas imprevistas – por exemplo, todas as máquinas virtuais de cluster de gerenciamento ficar inativo ao mesmo tempo – as informações de referência é adicionalmente persistentemente armazenado em cache em cada nó do conjunto de cluster, mesmo entre as reinicializações.
 
**Pergunta:** O cluster define o acesso de armazenamento com base no namespace desacelerar o desempenho de armazenamento em um conjunto de cluster? <br>
**Resposta:** Não. Namespace do conjunto de cluster oferece um namespace de referência de sobreposição dentro de um conjunto de cluster – conceitualmente como distribuídas arquivo sistema DFSN (Namespaces). E, ao contrário de DFSN, todos os metadados de referência do namespace conjunto de cluster são populado automaticamente e atualizado automaticamente em todos os nós sem nenhuma intervenção do administrador, portanto, não há quase nenhuma sobrecarga de desempenho no caminho de acesso de armazenamento. 

**Pergunta:** Como fazer backup metadados do conjunto de cluster? <br>
**Resposta:** Essa orientação é o mesmo que um Cluster de Failover. O Backup de estado do sistema fará backup o estado do cluster.  Por meio do Backup do Windows Server, você pode fazer restaurações de apenas de um nó cluster banco de dados (que nunca deve ser necessária por causa de um pouco de lógica de auto-recuperação temos) ou fazer uma restauração autoritativa para reverter o banco de dados de todo o cluster em todos os nós. No caso de conjuntos de cluster, a Microsoft recomenda fazer tal uma restauração autoritativa pela primeira vez no cluster membro e, em seguida, o grupo de gerenciamento se necessário.
