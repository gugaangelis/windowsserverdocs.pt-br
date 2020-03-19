---
title: Qualidade de Serviço do Armazenamento
ms.prod: windows-server
manager: dongill
ms.author: JGerend
ms.technology: storage-qos
ms.topic: get-started-article
ms.assetid: 8dcb8cf9-0e08-4fdd-9d7e-ec577ce8d8a0
author: kumudd
ms.date: 10/10/2016
ms.openlocfilehash: ed7d7ca4f41784f2ae12220eb2e30077e2467175
ms.sourcegitcommit: 056d355516f199e8a505c32b9aa685d0cde89e44
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79518741"
---
# <a name="storage-quality-of-service"></a>Qualidade de Serviço do Armazenamento

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

A Qualidade de serviço de armazenamento (QoS de armazenamento) no Windows Server 2016 fornece uma maneira de monitorar e gerenciar centralmente o desempenho de armazenamento de máquinas virtuais usando o Hyper-V e as funções de Servidor de Arquivos de Escalabilidade Horizontal. O recurso aprimora automaticamente a igualdade de recursos de armazenamento entre várias máquinas virtuais usando o mesmo cluster de servidor de arquivos e permite que as metas de desempenho mínimo e máximo baseadas em políticas sejam configuradas em unidades de IOPs normalizados.  

Você pode usar a QoS de armazenamento no Windows Server 2016 para realizar o seguinte:  

-   **Reduza problemas de vizinho barulhento.** Por padrão, a QoS de armazenamento garante que uma única máquina virtual não consuma todos os recursos de armazenamento e deixe outras máquinas virtuais sem largura de banda de armazenamento.  

-   **Monitore o desempenho de armazenamento de ponta a ponta.** Assim que as máquinas virtuais armazenadas em um Servidor de Arquivos de Escalabilidade Horizontal forem iniciadas, seu desempenho será monitorado. Os detalhes de desempenho de todas as máquinas virtuais em execução e a configuração do cluster de Servidor de Arquivos de Escalabilidade Horizontal podem ser exibidos em um único local  

-   **Gerenciar E/S de armazenamento de acordo com as necessidades de negócios de cargas de trabalho** As políticas de QoS de armazenamento definem os desempenhos mínimos e máximos para máquinas virtuais e garantem que eles sejam atendidos. Isso fornece desempenho consistente para as máquinas virtuais, mesmo em ambientes densos e provisionados em excesso. Se as políticas não forem atendidas, alertas estarão disponíveis para controlar quando as VMs estão fora da política ou têm políticas inválidas atribuídas.  

Este documento descreve como sua empresa pode se beneficiar da nova funcionalidade de QoS de armazenamento. Ele supõe que você tenha um conhecimento prático anterior sobre o Windows Server, Clustering de failover do Windows Server, Servidor de Arquivos de Escalabilidade Horizontal, Hyper-V e Windows PowerShell.

## <a name="BKMK_Overview"></a>Sobre  
Esta seção descreve os requisitos para usar a QoS de armazenamento, uma visão geral de uma solução definida pelo software usando a QoS de armazenamento, e uma lista de terminologias relacionadas a QoS de armazenamento.  

### <a name="BKMK_Requirements"></a>Requisitos de QoS de armazenamento  
A QoS de armazenamento oferece suporte a dois cenários de implantação:  

-   **Hyper-V usando um Servidor de Arquivos de Escalabilidade Horizontal** Esse cenário exige os seguintes itens:  

    -   Cluster de armazenamento que é um cluster de Servidor de Arquivos de Escalabilidade Horizontal  

    -   Cluster de computação que tem pelo menos um servidor com a função Hyper-V habilitada  

    Para a QoS de armazenamento, o Cluster de failover é necessário nos servidores de armazenamento, mas os servidores de computação não precisam estar em um cluster de failover. Todos os servidores (usados para computação e armazenamento) devem estar executando o Windows Server 2016.  

    Se você não tiver um cluster de Servidor de Arquivos de Escalabilidade Horizontal implantado para fins de avaliação, para obter instruções passo a passo para criar um usando servidores existentes ou máquinas virtuais, consulte [Armazenamento do Windows Server 2012 R2: passo a passo com espaços de armazenamento, escalabilidade para SMB e VHDX compartilhado (físico)](https://blogs.technet.com/b/josebda/archive/2013/07/31/windows-server-2012-r2-storage-step-by-step-with-storage-spaces-smb-scale-out-and-shared-vhdx-physical.aspx).  

-   **Hyper-V usando volumes compartilhados do cluster.** Esse cenário requer os seguintes itens:  

    -   Cluster de computação com a função Hyper-V habilitada  

    -   Hyper-V usando a Volumes Compartilhados de Cluster (CSV) para armazenamento  

Cluster de failover é necessário. Todos os servidores devem estar executando a mesma versão do Windows Server 2016.  

### <a name="BKMK_SolutionOverview"></a>Usando a QoS de armazenamento em uma solução de armazenamento definida por software  
A Qualidade de serviço do armazenamento é integrada na solução de armazenamento definida por software da Microsoft fornecida pelo Servidor de Arquivos de Escalabilidade Horizontal e pelo Hyper-V. O Servidor de Arquivos de Escalabilidade Horizontal expõe os compartilhamentos de arquivo para os servidores do Hyper-V usando o protocolo SMB3. Um novo Gerenciador de políticas foi adicionado ao cluster de Servidor de arquivos, que fornece o monitoramento central de desempenho de armazenamento.  

![QoS de armazenamento e Servidor de Arquivos de Escalabilidade Horizontal](media/overview-Clustering_SOFSStorageQoS.png)  

**Figura 1: usando a QoS de armazenamento em uma solução de armazenamento definida por software no Servidor de Arquivos de Escalabilidade Horizontal**  

À medida que os servidores do Hyper-V iniciam as máquinas virtuais, eles são monitorados pelo Gerenciador de políticas. O Gerenciador de políticas comunica a política de QoS de armazenamento e os limites ou reservas de volta para o servidor do Hyper-V, que controla o desempenho da máquina virtual conforme for apropriado.  

Quando há mudanças nas políticas de QoS de armazenamento ou nas demandas de desempenho feitas pelas máquinas virtuais, o Gerenciador de políticas notifica os servidores do Hyper-V para ajustar seu comportamento. Este ciclo de feedbacks garante que todos os VHDs das máquinas virtuais tenham desempenho consistente de acordo com as políticas de QoS de armazenamento conforme foi estabelecido.  

### <a name="BKMK_Glossary"></a>Glossário  

|Termo|Descrição|  
|--------|---------------|  
|IOPs normalizados|Todo o uso do armazenamento é medido em "IOPs normalizados".  Essa é uma contagem das operações de entrada/saída de armazenamento por segundo.  Qualquer E/S que tiver 8 KB ou menos é considerada como um E/S normalizada.  Qualquer E/S que tiver mais de 8 KB é tratada como várias E/S normalizadas. Por exemplo, uma solicitação de 256 KB é tratada como 32 IOPs normalizados.<br /><br />O Windows Server 2016 inclui a capacidade de especificar o tamanho usado para normalizar a E/S.  No cluster de armazenamento, o tamanho normalizado pode ser especificado e ficar em vigor no cluster de cálculos de normalização amplo.  O padrão permanece 8 KB.|  
|Fluxo|Cada identificador de arquivo aberto por um servidor do Hyper-V em um arquivo VHD ou VHDX é considerado um "fluxo". Se uma máquina virtual tiver dois discos rígidos virtuais anexados, ela terá 1 fluxo para o cluster de servidor de arquivo por arquivo. Se um VHDX for compartilhado com várias máquinas virtuais, ele terá 1 fluxo por máquina virtual.|  
|InitiatorName|Nome da máquina virtual que é relatada para o Servidor de Arquivos de Escalabilidade Horizontal para cada fluxo.|  
|InitiatorID|Um identificador correspondente à ID da máquina virtual.  Isso sempre pode ser usado para identificar exclusivamente as máquinas virtuais de fluxos individuais, mesmo se as máquinas virtuais tiverem o mesmo InitiatorName.|  
|Política|As políticas de QoS de armazenamento são armazenadas no banco de dados do cluster e têm as seguintes propriedades: PolicyId, MinimumIOPS, MaximumIOPS, ParentPolicy e PolicyType.|  
|PolicyId|Identificador exclusivo para uma política.  Ele é gerado por padrão, mas pode ser especificado, se isso for desejado.|  
|MinimumIOPS|IOPs normalizados mínimos que serão fornecidos por uma política.  Também conhecidos como "Reserva".|  
|MaximumIOPS|IOPs normalizados máximos que serão limitados por uma política.  Também conhecidos como "Limite".|  
|Agregados |Um tipo de política onde o MinimumIOPS, o MaximumIOPS e a largura de banda especificados são compartilhados entre todos os fluxos atribuídos à política. Todos os VHDs atribuídos à política nesse sistema de armazenamento tem uma única alocação de largura de banda de E/S para eles para compartilhamento de todos.|  
|Dedicado|Um tipo de política onde os IOPs mínimo e máximo, e a largura de banda são gerenciados para VHD/VHDx individuais.|  

## <a name="BKMK_SetUpQoS"></a>Como configurar a QoS de armazenamento e monitorar o desempenho básico  
Esta seção descreve como habilitar o novo recurso de QoS de armazenamento e como monitorar o desempenho de armazenamento sem a aplicação de políticas personalizadas.  

### <a name="BKMK_SetupStorageQoSonStorageCluster"></a>Configurar a QoS de armazenamento em um cluster de armazenamento  
Esta seção descreve como habilitar a QoS de armazenamento em um cluster de failover e um Servidor de Arquivos de Escalabilidade Horizontal novo ou existente que está executando o Windows Server 2016.  

#### <a name="set-up-storage-qos-on-a-new-installation"></a>Configurar a QoS de armazenamento em uma nova instalação  
Se você tiver configurado um novo cluster de failover e um Volume Compartilhado Clusterizado (CSV) no Windows Server 2016, o recurso de QoS de armazenamento será configurado automaticamente.  

#### <a name="verify-storage-qos-installation"></a>Verificar a instalação da QoS de armazenamento  
Depois de criar um Cluster de Failover e configurar um disco de CSV, o **Recurso de QoS de armazenamento** será exibido como um Recurso principal do cluster e estará visível no Gerenciador de Cluster de Failover e no Windows PowerShell. A intenção é que o sistema de cluster de failover gerenciará esse recurso e você não terá que fazer nada em relação a esse recurso.  Vamos exibi-lo no Gerenciador de Cluster de Failover e no PowerShell para ser consistente com outros recursos de sistema de cluster de failover como o novo Serviço de integridade.  

![O recurso de QoS de armazenamento aparece em Recursos Principais de Cluster](media/overview-Clustering_StorageQoSFCM.png)  

**Figura 2: recurso de QoS de armazenamento exibido como um recurso de núcleo de cluster no Gerenciador de Cluster de Failover**  

Use o seguinte cmdlet do PowerShell para exibir o status do Recurso de QoS de armazenamento.  

```PowerShell  
PS C:\> Get-ClusterResource -Name "Storage Qos Resource"  

Name                   State      OwnerGroup        ResourceType                 
----                   -----      ----------        ------------                 
Storage Qos Resource   Online     Cluster Group     Storage QoS Policy Manager  
```  

### <a name="BKMK_SetupStorageQoSonComputeCluster"></a>Configurar a QoS de armazenamento em um cluster de computação  
A função do Hyper-V no Windows Server 2016 tem suporte interno para o QoS de armazenamento e é habilitada por padrão.  

#### <a name="install-remote-administration-tools-to-manage-storage-qos-policies-from-remote-computers"></a>Instalar as Ferramentas de administração remota para gerenciar as políticas de QoS de armazenamento de computadores remotos  
Você pode gerenciar as políticas de QoS de armazenamento e monitorar os fluxos de hosts de computação usando as Ferramentas de administração de servidor remoto.  Elas estão disponíveis como recursos opcionais em todas as instalações do Windows Server 2016 e podem ser baixadas separadamente para Windows 10 no site [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=45520).

O recurso opcional **RSAT-Clustering** inclui o módulo do Windows PowerShell para gerenciamento remoto do Cluster de failover, incluindo o QoS de armazenamento.  

-   Windows PowerShell: Add-WindowsFeature RSAT-Clustering  

O recurso opcional **RSAT-Hyper-V-Tools** inclui o módulo do Windows PowerShell para o gerenciamento remoto do Hyper-V.  

-   Windows PowerShell: Add-WindowsFeature RSAT-Hyper-V-Tools  

#### <a name="deploy-virtual-machines-to-run-workloads-for-testing"></a>Implantar máquinas virtuais para executar cargas de trabalho de teste  
Você precisará de algumas máquinas virtuais armazenadas no Servidor de Arquivos de Escalabilidade Horizontal com cargas de trabalho relevantes.  Para obter algumas dicas sobre como simular a carga e fazer alguns testes de estresse, consulte a página a seguir para obter uma ferramenta recomendada (DiskSpd) e um exemplo de uso: [DiskSpd, PowerShell e desempenho do armazenamento: medir IOPs, taxa de transferência e latência para discos locais e compartilhamentos de arquivos de SMB.](https://blogs.technet.com/b/josebda/archive/2014/10/13/diskspd-powershell-and-storage-performance-measuring-iops-throughput-and-latency-for-both-local-disks-and-smb-file-shares.aspx)  

Os cenários de exemplo mostrados neste guia incluem cinco máquinas virtuais. BuildVM1, BuildVM2, BuildVM3 e BuildVM4 estão executando uma carga de trabalho da área de trabalho com demandas de armazenamento baixas a moderadas. TestVm1 está executando um parâmetro de comparação de processamento de transações online com demanda de armazenamento alta.  

### <a name="view-current-storage-performance-metrics"></a>Exibir métricas de desempenho de armazenamento atuais  
Esta seção inclui:  

-   Como consultar os fluxos usando o cmdlet `Get-StorageQosFlow`.  

-   Como exibir o desempenho de um volume usando o cmdlet `Get-StorageQosVolume`.  

#### <a name="query-flows-using-the-get-storageqosflow-cmdlet"></a>Fluxos de consulta usando o cmdlet Get-StorageQosFlow  

O cmdlet Get-StorageQosFlow mostra todos os fluxos atuais iniciados por servidores do Hyper-V. Todos os dados são coletados pelo cluster de Servidor de Arquivos de Escalabilidade Horizontal; portanto, o cmdlet pode ser usado em qualquer nó do cluster de Servidor de Arquivos de Escalabilidade Horizontal ou em um servidor remoto usando o parâmetro -CimSession.  

**O comando de exemplo a seguir mostra como exibir todos os arquivos abertos pelo Hyper-V no servidor usando Get-StorageQoSFlow**.  

```PowerShell
PS C:\> Get-StorageQosFlow  

InitiatorName    InitiatorNodeNam StorageNodeName  FilePath        Status  
                 e  
-------------    ---------------- ---------------  --------        ------  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM1         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM2         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
```  

O comando de exemplo a seguir está formatado para mostrar o nome da máquina virtual, o nome do host do Hyper-V, os IOPS e o nome do arquivo VHD, classificados por IOPS.  

```PowerShell  
PS C:\> Get-StorageQosFlow | Sort-Object StorageNodeIOPs -Descending | ft InitiatorName, @{Expression={$_.InitiatorNodeName.Substring(0,$_.InitiatorNodeName.IndexOf('.'))};Label="InitiatorNodeName"}, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName InitiatorNodeName StorageNodeIOPs Status File  
------------- ----------------- --------------- ------ ----  
WinOltp1      plang-c1                     3482     Ok IOMETER.VHDX  
BuildVM2      plang-c2                      544     Ok BUILDVM2.VHDX  
BuildVM1      plang-c2                      497     Ok BUILDVM1.VHDX  
BuildVM4      plang-c2                      316     Ok BUILDVM4.VHDX  
BuildVM3      plang-c2                      296     Ok BUILDVM3.VHDX  
BuildVM4      plang-c2                      195     Ok WIN8RTM_ENTERPRISE_VL_BU...  
TR20-VMM      plang-z400                    156     Ok DATA1.VHDX  
BuildVM3      plang-c2                       81     Ok WIN8RTM_ENTERPRISE_VL_BU...  
WinOltp1      plang-c1                       65     Ok BOOT.VHDX  
                                             18     Ok DefaultFlow  
                                             12     Ok DefaultFlow  
WinOltp1      plang-c1                        4     Ok 9914.0.AMD64FRE.WINMAIN....  
TR20-VMM      plang-z400                      4     Ok DATA2.VHDX  
TR20-VMM      plang-z400                      3     Ok BOOT.VHDX  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
```  

O comando de exemplo a seguir mostra como filtrar fluxos com base no InitiatorName para localizar com facilidade o desempenho de armazenamento e as configurações para uma máquina virtual específica.  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName BuildVm1 | Format-List

FilePath           : C:\ClusterStorage\Volume2\SHARES\TWO\BUILDWORKLOAD\BUILDVM1.V  
                     HDX  
FlowId             : ebfecb54-e47a-5a2d-8ec0-0940994ff21c  
InitiatorId        : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
InitiatorIOPS      : 464  
InitiatorLatency   : 26.2684  
InitiatorName      : BuildVM1  
InitiatorNodeName  : plang-c2.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 500  
PolicyId           : b145e63a-3c9e-48a8-87f8-1dfc2abfe5f4  
Reservation        : 500  
Status             : Ok  
StorageNodeIOPS    : 475  
StorageNodeLatency : 7.9725  
StorageNodeName    : plang-fs1.plang.nttest.microsoft.com  
TimeStamp          : 2/12/2015 2:58:49 PM  
VolumeId           : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName     :  
MaximumIops        : 500  
MinimumIops        : 500  
```  

Os dados retornados pelo cmdlet `Get-StorageQosFlow` incluem:  

-   O nome de host do Hyper-V (InitiatorNodeName).  

-   O nome da máquina virtual e sua identificação (InitiatorName e InitiatorId)  

-   O desempenho médio recente conforme observado pelo host do Hyper-V para o disco virtual (InitiatorIOPS, InitiatorLatency)  

-   O desempenho médio recente conforme observado pelo cluster de armazenamento para o disco virtual (StorageNodeIOPS, StorageNodeLatency)  

-   A política atual que está sendo aplicada para o arquivo, se houver, e a configuração resultante (PolicyId, Reserva, Limite)  

-   O status da política  

    -   **OK**: não há nenhum problema  

    -   InsufficientThroughput: uma política é aplicada, mas o IOPs mínimo não pode ser entregue.  Isso poderá acontecer se o mínimo para uma VM, ou todas as máquinas virtuais juntas, for maior do que o volume de armazenamento pode oferecer.  

    -   **UnknownPolicyId** uma política foi atribuída à máquina virtual no host do Hyper-V, mas está ausente do servidor de arquivos.  Essa política deve ser removida da configuração de máquina virtual ou uma política de correspondência deve ser criada no cluster de servidor de arquivos.  

#### <a name="view-performance-for-a-volume-using-get-storageqosvolume"></a>Exibir o desempenho de um volume usando Get-StorageQosVolume  
As métricas de desempenho de armazenamento também são coletadas em um nível de volume por armazenamento, além das métricas de desempenho por fluxo.  Isso facilita ver a utilização média total de IOPs normalizados, latência e limites de agregação e reservas aplicadas a um volume.  

```PowerShell
PS C:\> Get-StorageQosVolume | Format-List  

Interval       : 300000  
IOPS           : 0  
Latency        : 0  
Limit          : 0  
Reservation    : 0  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 434f561f-88ae-46c0-a152-8c6641561504  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 0  

Interval       : 300000  
IOPS           : 1097  
Latency        : 3.1524  
Limit          : 0  
Reservation    : 1568  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 1568  

Interval       : 300000  
IOPS           : 5354  
Latency        : 6.5084  
Limit          : 0  
Reservation    : 781  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 781  
```  

## <a name="BKMK_CreateQoSPolicies"></a>Como criar e monitorar políticas de QoS de armazenamento  
Esta seção descreve como criar políticas de QoS de armazenamento, aplicar essas políticas a máquinas virtuais e monitorar um cluster de armazenamento depois que as políticas forem aplicadas.  

### <a name="create-storage-qos-policies"></a>Criar políticas de QoS de armazenamento  
As políticas de QoS de armazenamento são definidas e gerenciadas no cluster de Servidor de Arquivos de Escalabilidade Horizontal.  Você pode criar quantas políticas forem necessárias para implantações flexíveis (até 10.000 por cluster de armazenamento).  

Cada arquivo VHD/VHDX atribuído a uma máquina virtual pode ser configurado com uma política. Arquivos e máquinas virtuais diferentes podem usar a mesma política ou cada um pode ser configurado com políticas separadas.  Se vários arquivos VHD/VHDX ou várias máquinas virtuais forem configurados com a mesma política, eles serão agregados juntos e compartilharão o MinimumIOPS e MaximumIOPS igualmente. Se você usar políticas separadas para várias máquinas virtuais ou arquivos VHD/VHDX, o mínimo e máximo serão rastreados separadamente para cada um.  

Se você criar várias políticas semelhantes para diferentes máquinas virtuais e as máquinas virtuais tiverem demandas de armazenamento iguais, elas receberão um compartilhamento semelhante de IOPs.  Se uma máquina virtual exigir mais e outra menos, em seguida, os IOPs acompanharão essa demanda.  

### <a name="types-of-storage-qos-policies"></a>Tipos de políticas de QoS de armazenamento  
Há dois tipos de políticas: Agregada (anteriormente conhecido como SingleInstance) e Dedicada (anteriormente conhecido como MultiInstance). As políticas agregadas aplicam máximos e mínimos para o conjunto combinado de arquivos VHD/VHDX e máquinas virtuais em que elas se aplicam. Na verdade, eles compartilham um conjunto especificado de IOPS e largura de banda. As políticas Dedicadas aplicam os valores mínimo e máximo para cada VHD/VHDx, separadamente. Isso facilita a criação de uma única política que aplica limites semelhante a vários arquivos VHD/VHDx.  

Por exemplo, se você criar uma política Agregadas com um mínimo de 300 IOPs e um máximo de 500 IOPs. Se você aplicar esta política a cinco arquivos VHD/VHDx diferentes, estará garantindo que os cinco arquivos VHD/VHDx combinados tenham ao menos 300 IOPs (se houver demanda e o sistema de armazenamento puder fornecer esse desempenho) e não mais que 500 IOPs. Se os arquivos VHD/VHDx tiverem demanda alta semelhante para IOPs e o sistema de armazenamento puder acompanhar, cada arquivo VHD/VHDx receberá cerca de 100 IOPs.  

No entanto, se você criar uma política Dedicada com limites semelhantes e aplicá-la a arquivos VHD/VHDx em 5 máquinas virtuais diferentes, cada máquina virtual receberá pelo menos 300 IOPs e não mais que 500 IOPs. Se as máquinas virtuais tiverem demanda alta semelhante para IOPs e o sistema de armazenamento puder acompanhar, cada máquina virtual receberá cerca de 500 IOPs. .  Se uma das máquinas virtuais tiver vários arquivos VHD/VHDx com a mesma política MulitInstance configurada, eles compartilharão o limite para que a E/S total da VM dos arquivos com essa política não exceda os limites.  

Portanto, se você tiver um grupo de arquivos VHD/VHDx que você quiser exibir as mesmas características de desempenho e não quiser o trabalho de criar várias políticas semelhantes, poderá usar uma única política Dedicada e aplicar aos arquivos de cada máquina virtual.

Mantenha o número de arquivos VHD/VHDx atribuídos a uma única política agregada para 20 ou menos.  Esse tipo de política foi criado para fazer a agregação com algumas VMs em um cluster.

### <a name="create-and-apply-a-dedicated-policy"></a>Criar e aplicar uma política Dedicada  
Primeiro, use o cmdlet `New-StorageQosPolicy` para criar uma política no Servidor de Arquivos de Escalabilidade Horizontal, conforme mostrado no exemplo a seguir:  

```PowerShell
$desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  
```  

Em seguida, aplique-o nos discos rígidos das máquinas virtuais apropriadas no servidor do Hyper-V.  Observe o PolicyId da etapa anterior ou armazene-o em uma variável em seus scripts.  

No Servidor de Arquivos de Escalabilidade Horizontal, usando o PowerShell, crie uma política de QoS de armazenamento e obtenha a identificação da política, conforme mostrado no exemplo a seguir:  

```PowerShell
PS C:\> $desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  

C:\> $desktopVmPolicy.PolicyId  

Guid  
----  
cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

No servidor do Hyper-V, usando o PowerShell, defina a política de QoS de armazenamento usando a identificação da política, conforme mostrado no exemplo a seguir:  

```PowerShell
Get-VM -Name Build* | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

### <a name="confirm-that-the-policies-are-applied"></a>Confirmar que as políticas são aplicadas  
Use o cmdlet `Get-StorageQosFlow` do PowerShell para confirmar se o MinimumIOPs e MaximumIOPs foram aplicados ao fluxo apropriado conforme mostrado no exemplo a seguir.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName |  
 ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
BuildVM1          Ok         100         200             250     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             251     Ok BUILDVM2.VHDX  
BuildVM3          Ok         100         200             252     Ok BUILDVM3.VHDX  
BuildVM4          Ok         100         200             233     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               1     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               5     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666               4     Ok BOOT.VHDX  
WinOltp1          Ok           0           0               0     Ok 9914.0.AMD6...  
WinOltp1          Ok           0           0            5166     Ok IOMETER.VHDX  
WinOltp1          Ok           0           0               0     Ok BOOT.VHDX  
```  

No servidor do Hyper-V, você também pode usar o script **Get-VMHardDiskDrivePolicy.ps1** fornecido para ver qual política é aplicada a um disco rígido virtual.  

```PowerShell
PS C:\> Get-VM -Name BuildVM1 | Get-VMHardDiskDrive | Format-List  

Path                          : \\plang-fs.plang.nttest.microsoft.com\two\BuildWorkload  
                                \BuildVM1.vhdx  
DiskNumber                    :  
MaximumIOPS                   : 0  
MinimumIOPS                   : 0  
QoSPolicyID                   : cd6e6b87-fb13-492b-9103-41c6f631f8e0  
SupportPersistentReservations : False  
ControllerLocation            : 0  
ControllerNumber              : 0  
ControllerType                : IDE  
PoolName                      : Primordial  
Name                          : Hard Drive  
Id                            : Microsoft:AE4E3DD0-3BDE-42EF-B035-9064309E6FEC\83F8638B  
                                -8DCA-4152-9EDA-2CA8B33039B4\0\0\D  
VMId                          : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
VMName                        : BuildVM1  
VMSnapshotId                  : 00000000-0000-0000-0000-000000000000  
VMSnapshotName                :  
ComputerName                  : PLANG-C2  
IsDeleted                     : False  
```  

### <a name="query-for-storage-qos-policies"></a>Consultar as políticas de QoS de armazenamento  
`Get-StorageQosPolicy` lista todas as políticas configuradas e seu status em uma Servidor de Arquivos de Escalabilidade Horizontal.  

```PowerShell
PS C:\> Get-StorageQosPolicy  

Name                 MinimumIops          MaximumIops          Status  
----                 -----------          -----------          ------  
Default              0                    0                    Ok  
Limit500             0                    500                  Ok  
SilverVm             500                  500                  Ok  
Desktop              100                  200                  Ok  
Limit500             0                    0                    Ok  
VMM                  100                  2000                 Ok  
Vdi                  1                    100                  Ok  
```  

O status pode ser alterado ao longo do tempo com base no desempenho do sistema.  

-   **OK** todos os fluxos que usam essa política estão recebendo seus MinimumIOPs solicitados.  

-   **InsufficientThroughput** um ou mais fluxos que usam essa política não estão recebendo os IOPs mínimos  

Você também pode redirecionar uma política para `Get-StorageQosPolicy` a fim de obter o status de todos os fluxos configurados para usar a política da seguinte maneira:  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name Desktop | Get-StorageQosFlow | ft InitiatorName, *IOPS, Status, FilePath -AutoSize  

InitiatorName MaximumIops MinimumIops InitiatorIOPS StorageNodeIOPS Status FilePat  
                                                                           h  
------------- ----------- ----------- ------------- --------------- ------ -------  
BuildVM4              100          50           187              17     Ok C:\C...  
BuildVM3              100          50           194              25     Ok C:\C...  
BuildVM1              200         100           195             196     Ok C:\C...  
BuildVM2              200         100           193             192     Ok C:\C...  
BuildVM4              200         100           187             169     Ok C:\C...  
BuildVM3              200         100           194             169     Ok C:\C...  
```  

### <a name="create-an-aggregated-policy"></a>Criar uma política agregada  
As políticas agregadas podem ser usadas se você quiser vários discos rígidos virtuais para compartilhar um único pool de IOPs e largura de banda.  Por exemplo, se você aplicar a mesma política agregada a discos rígidos de duas máquinas virtuais, o mínimo será dividido entre eles de acordo com a demanda.  Ambos os discos terão a garantia de um mínimo combinado e juntos eles não excederão os IOPs máximos ou largura de banda especificados.  

A mesma abordagem também pode ser usada para fornecer uma única alocação para todos os arquivos VHD/VHDx para as máquinas virtuais que compõem um serviço ou que pertencem a um locatário em um ambiente multi-hospedado.  

Não há nenhuma diferença no processo de criar políticas dedicadas e agregadas além do PolicyType especificado.  

O exemplo a seguir mostra como criar uma política de QoS de armazenamento agregada e obter sua policyID em um Servidor de Arquivos de Escalabilidade Horizontal:  

```PowerShell
PS C:\> $highPerf = New-StorageQosPolicy -Name SqlWorkload -MinimumIops 1000 -MaximumIops 5000 -PolicyType Aggregated  
[plang-fs]: PS C:\Users\plang\Documents> $highPerf.PolicyId  

Guid  
----  
7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

O exemplo a seguir mostra como aplicar a política de QoS de armazenamento no servidor do Hyper-V usando a policyID obtida no exemplo anterior:  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID 7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

O exemplo a seguir mostra como exibir os efeitos da política de QoS de armazenamento do servidor de arquivos:  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | format-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | for  
mat-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
```  

Cada disco rígido virtual terá os valores MinimumIOPs, MaximumIOPs e MaximumIobandwidth ajustados com base na sua carga.  Isso garante que a quantidade total de largura de banda usada para o grupo de discos permanece dentro do intervalo definido pela política.  No exemplo acima, os dois primeiros discos estão ociosos e o terceiro tem permissão para usar o IOPs máximo.  Se os dois primeiros discos começarem a emitir E/S novamente, o IOPs máximo do terceiro disco será reduzido automaticamente.  

### <a name="modify-an-existing-policy"></a>Modificar uma política existente  
As propriedades Name, MinimumIOPs,  MaximumIOPs, e MaximumIoBandwidth podem ser alteradas depois que uma política é criada.  No entanto, o tipo de política (Agregada/Dedicada) não pode ser alterado depois que a política é criada.  

O cmdlet do Windows PowerShell a seguir mostra como alterar a propriedade MaximumIOPs para uma política existente:  

```PowerShell
[DBG]: PS C:\demoscripts>> Get-StorageQosPolicy -Name SqlWorkload | Set-StorageQosPolicy -MaximumIops 6000  
```  

O cmdlet a seguir verifica a alteração:  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
SqlWorkload             1000                   6000                   Ok    

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageQosPolicy -Name SqlWorkload | Get-Storag  
eQosFlow | Format-Table InitiatorName, PolicyId, MaximumIops, MinimumIops, StorageNodeIops -A  
utoSize  

InitiatorName PolicyId                             MaximumIops MinimumIops StorageNodeIops  
------------- --------                             ----------- ----------- ---------------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        6000        1000            4507  
```  

## <a name="BKMK_KnownIssues"></a>Como identificar e resolver problemas comuns  
Esta seção descreve como localizar as máquinas virtuais com as políticas de QoS de armazenamento inválidas, como recriar uma política de correspondência, como remover uma política de uma máquina virtual e como identificar as máquinas virtuais que não atendem aos requisitos da política de QoS de armazenamento.  

### <a name="BKMK_FindingVMsWithInvalidPolicies"></a>Identificar máquinas virtuais com políticas inválidas  

Se uma política for excluída do servidor de arquivos antes de ser removida de uma máquina virtual, a máquina virtual continuará a ser executada como se nenhuma política tivesse sido aplicada.  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload | Remove-StorageQosPolicy  

Confirm  
Are you sure you want to perform this action?  
Performing the operation "DeletePolicy" on target "MSFT_StorageQoSPolicy (PolicyId =  
"7e2f3e73-1ae4-4710-8219-0769a4aba072")".  
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"):  
```  

O status para os fluxos agora mostrará "UnknownPolicyId"  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName          Status MinimumIops MaximumIops StorageNodeIOPs          Status File  
-------------          ------ ----------- ----------- ---------------          ------ ----  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0              10              Ok Def...  
                           Ok           0           0              13              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
BuildVM1                   Ok         100         200             193              Ok BUI...  
BuildVM2                   Ok         100         200             196              Ok BUI...  
BuildVM3                   Ok          50          64              17              Ok WIN...  
BuildVM3                   Ok          50         136             179              Ok BUI...  
BuildVM4                   Ok          50         100              23              Ok WIN...  
BuildVM4                   Ok         100         200             173              Ok BUI...  
TR20-VMM                   Ok          33         666               2              Ok DAT...  
TR20-VMM                   Ok          25         975               3              Ok DAT...  
TR20-VMM                   Ok          75        1025              12              Ok BOO...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId 991...  
WinOltp1      UnknownPolicyId           0           0            4926 UnknownPolicyId IOM...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId BOO...  
```  

#### <a name="BKMK_RecreateMatchingPolicy"></a>Recriar uma política de QoS de armazenamento correspondente  
Se uma política foi removida acidentalmente, você poderá criar uma nova usando a PolicyId antiga.  Primeiro, obtenha a PolicyId necessária  

```PowerShell
PS C:\> Get-StorageQosFlow -Status UnknownPolicyId | ft InitiatorName, PolicyId -AutoSize  

InitiatorName PolicyId  
------------- --------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

Em seguida, crie uma nova política usando essa PolicyId  

```PowerShell
PS C:\> New-StorageQosPolicy -PolicyId 7e2f3e73-1ae4-4710-8219-0769a4aba072 -PolicyType Aggregated -Name RestoredPolicy -MinimumIops 100 -MaximumIops 2000  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
RestoredPolicy          100                    2000                   Ok  
```  

Por fim, verifique se ela foi aplicada.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               8     Ok DefaultFlow  
                  Ok           0           0               9     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
BuildVM1          Ok         100         200             192     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             193     Ok BUILDVM2.VHDX  
BuildVM3          Ok          50         100              24     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM3          Ok         100         200             166     Ok BUILDVM3.VHDX  
BuildVM4          Ok          50         100              12     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM4          Ok         100         200             178     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666              10     Ok BOOT.VHDX  
WinOltp1          Ok          25         500               0     Ok 9914.0.AMD64FRE.WINMA...  
```  

#### <a name="BKMK_RemovePolicyFromVM"></a>Remover políticas de QoS de armazenamento  

Se a política tiver sido removida intencionalmente ou se uma máquina virtual tiver sido importada com uma política que você não precisa, ela pode ser removida.  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID $null  
```  

Depois que a PolicyId é removida das configurações do disco rígido virtual, o status será "Ok" e nenhum mínimo ou máximo será aplicado.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ----------- ----------- --------------- ------ ----  
                        0           0               0     Ok DefaultFlow  
                        0           0              16     Ok DefaultFlow  
                        0           0              12     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
BuildVM1              100         200             197     Ok BUILDVM1.VHDX  
BuildVM2              100         200             192     Ok BUILDVM2.VHDX  
BuildVM3                9           9              23     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM3               91         191             171     Ok BUILDVM3.VHDX  
BuildVM4                8           8              18     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM4               92         192             163     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               2     Ok DATA2.VHDX  
TR20-VMM               33         666               1     Ok DATA1.VHDX  
TR20-VMM               33         666               5     Ok BOOT.VHDX  
WinOltp1                0           0               0     Ok 9914.0.AMD64FRE.WINMAIN.1412...  
WinOltp1                0           0            1811     Ok IOMETER.VHDX  
WinOltp1                0           0               0     Ok BOOT.VHDX  
```  

### <a name="BKMK_VMsThatDoNotMeetStorageQoSPoilicies"></a>Localizar máquinas virtuais que não estão atendendo às políticas de QoS de armazenamento  
O status **InsufficientThroughput** é atribuído a qualquer fluxo que:  

-   Tem um IOPs mínimo definido configurado pela política; e  

-   Está iniciando a E/S com uma taxa que atende ou excede o mínimo; e  

-   Não está obtendo a taxa mínima de IOP  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs                 Status File  
------------- ----------- ----------- ---------------                 ------ ----  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0              15                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
BuildVM3               50         100              20                     Ok WIN8RTM_ENTE...  
BuildVM3              100         200             174                     Ok BUILDVM3.VHDX  
BuildVM4               50         100              11                     Ok WIN8RTM_ENTE...  
BuildVM4              100         200             188                     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               3                     Ok DATA1.VHDX  
TR20-VMM               78        1032             180                     Ok BOOT.VHDX  
TR20-VMM               22         968               4                     Ok DATA2.VHDX  
WinOltp1             3750        5000               0                     Ok 9914.0.AMD64...  
WinOltp1            15000       20000           11679 InsufficientThroughput IOMETER.VHDX  
WinOltp1             3750        5000               0                     Ok BOOT.VHDX  
```  

Você pode determinar fluxos para qualquer status, inclusive **InsufficientThroughput** conforme mostrado no exemplo a seguir:  

```PowerShell
PS C:\> Get-StorageQosFlow -Status InsufficientThroughput | fl  

FilePath           : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
FlowId             : 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
InitiatorId        : 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
InitiatorIOPS      : 12168  
InitiatorLatency   : 22.983  
InitiatorName      : WinOltp1  
InitiatorNodeName  : plang-c1.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 20000  
PolicyId           : 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
Reservation        : 15000  
Status             : InsufficientThroughput  
StorageNodeIOPS    : 12181  
StorageNodeLatency : 22.0514  
StorageNodeName    : plang-fs2.plang.nttest.microsoft.com  
TimeStamp          : 2/13/2015 12:07:30 PM  
VolumeId           : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName     :  
MaximumIops        : 20000  
MinimumIops        : 15000  
```  

## <a name="BKMK_Health"></a>Monitorar a integridade usando QoS de armazenamento  
O novo serviço de integridade simplifica o monitoramento do Cluster de armazenamento, fornecendo um local único para verificar se há eventos acionáveis em qualquer um dos nós. Esta seção descreve como monitorar a integridade do seu cluster de armazenamento usando o cmdlet `debug-storagesubsystem`.  

### <a name="view-storage-status-with-debug-storagesubsystem"></a>Exibir o status de armazenamento com Debug-StorageSubSystem  
Os espaços de armazenamento clusterizados também fornecem informações sobre a integridade do cluster de armazenamento em um único local. Isso pode ajudar os administradores a rapidamente identificarem problemas atuais em implantações de armazenamento e monitorar os problemas que chegam ou são ignorados.  

#### <a name="vm-with-invalid-policy"></a>VM com política inválida  
VMs com políticas inválidas também são relatadas por meio do monitoramento de integridade do subsistema de armazenamento.  Aqui está um exemplo do mesmo estado, conforme descrito na seção [Localizar VMs com políticas inválidas](#BKMK_FindingVMsWithInvalidPolicies) deste documento.  

```PowerShell
C:\> Get-StorageSubSystem -FriendlyName Clustered* | Debug-StorageSubSystem  

EventTime                 :  
FaultId                   : 0d16d034-9f15-4920-a305-f9852abf47c3  
FaultingObject            :  
FaultingObjectDescription : Storage QoS Policy 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
FaultingObjectLocation    :  
FaultType                 : Storage QoS policy used by consumer does not exist.  
PerceivedSeverity         : Minor  
Reason                    : One or more storage consumers (usually Virtual Machines) are  
                            using a non-existent policy with id  
                            5d1bf221-c8f0-4368-abcf-aa139e8a7c72. Consumer details:  

                            Flow ID: 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
                            Initiator ID: 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
                            Initiator Name: WinOltp1  
                            Initiator Node: plang-c1.plang.nttest.microsoft.com  
                            File Path:  
                            C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
RecommendedActions        : {Reconfigure the storage consumers (usually Virtual Machines)  
                            to use a valid policy., Recreate any missing Storage QoS  
                            policies.}  
PSComputerName            :  
```  

#### <a name="lost-redundancy-for-a-storage-spaces-virtual-disk"></a>Perda de redundância para um disco virtual de espaços de armazenamento  

Neste exemplo, um espaço de armazenamento clusterizado tem um disco virtual criado como um espelho de três vias.  Um disco com falha foi removido do sistema, mas não foi adicionado um disco de substituição.  O subsistema de armazenamento está relatando uma perda de redundância com HealthStatus **Aviso**, mas OperationalStatus "**OK** porque o volume ainda está online.  

```PowerShell
PS C:\> Get-StorageSubSystem -FriendlyName Clustered*  

FriendlyName                   HealthStatus                   OperationalStatus  
------------                   ------------                   -----------------  
Clustered Windows Storage o... Warning                        OK  

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageSubSystem -FriendlyName Clustered* | Deb  
ug-StorageSubSystem  

EventTime                 :  
FaultId                   : dfb4b672-22a6-4229-b2ed-c29d7485bede  
FaultingObject            :  
FaultingObjectDescription : Virtual disk 'Two'  
FaultingObjectLocation    :  
FaultType                 : VirtualDiskDegradedFaultType  
PerceivedSeverity         : Minor  
Reason                    : Virtual disk 'Two' does not have enough redundancy remaining to  
                            successfully repair or regenerate its data.  
RecommendedActions        : {Rebalance the pool, replace failed physical disks, or add new  
                            physical disks to the storage pool, then repair the virtual  
                            disk.}  
PSComputerName            :  
```  

### <a name="sample-script-for-continuous-monitoring-of-storage-qos"></a>Script de exemplo para monitoramento contínuo de QoS de armazenamento  

Esta seção inclui um script de exemplo que mostra como as falhas comuns podem ser monitoradas usando o script de WMI.  Ele foi projetado como uma parte inicial para os desenvolvedores recuperarem os eventos de integridade em tempo real.  

**Script de exemplo:**  

```PowerShell
param($cimSession)  
# Register and display events  
Register-CimIndicationEvent -Namespace root\microsoft\windows\storage -ClassName msft_storagefaultevent -CimSession $cimSession  

while ($true)  
{  
     $e = (Wait-Event)  
     $e.SourceEventArgs.NewEvent  
     Remove-Event $e.SourceIdentifier  
}  
```  

## <a name="frequently-asked-questions"></a>Perguntas frequentes  

### <a name="how-do-i-retain-a-storage-qos-policy-being-enforced-for-my-virtual-machine-if-i-move-its-vhdvhdx-files-to-another-storage-cluster"></a>Como posso manter uma política de QoS de armazenamento que está sendo imposta para minha máquina virtual se mover meus arquivos VHD/VHDx para outro cluster de armazenamento  

A configuração no arquivo VHD/VHDx que especifica a política é o GUID de uma ID de política.  Quando uma política é criada, o GUID pode ser especificado usando o parâmetro **PolicyID**.  Se esse parâmetro não for especificado, um GUID aleatório será criado.  Portanto, você pode obter o PolicyID no cluster de armazenamento onde as VMs atualmente armazenam seus arquivos VHD/VHDx e criar uma política idêntica no cluster de armazenamento de destino e, em seguida, especificar que ela seja criada com o mesmo GUID.  Quando os arquivos de máquinas virtuais são movidos para novos clusters de armazenamento, a política com o mesmo GUID estará em vigor.  

O System Center Virtual Machine Manager pode ser usado para aplicar políticas de vários clusters de armazenamento, o que facilita muito esse cenário.  
### <a name="if-i-change-the-storage-qos-policy-why-dont-i-see-it-take-effect-immediately-when-i-run-get-storageqosflow"></a>Se eu tiver alterado a política de QoS, por que ela não entrou em vigor imediatamente ao executar Get-StorageQoSFlow  

Se você tiver um fluxo que está atingindo o máximo de uma política e alterar a política para torná-la maior ou menor e, depois determinar imediatamente a latência/IOPS/largura de banda dos fluxos usando os cmdlets do PowerShell, levará até 5 minutos para ver os efeitos completos da mudança da política nos fluxos.  Os novos limites entrarão em vigor em poucos segundos, mas o cmdlet **Get-StorgeQoSFlow** do PowerShell usa uma média de cada contador usando uma janela deslizante de 5 minutos.  Caso contrário, se estiver mostrando um valor atual e você executar o cmdlet do PowerShell várias vezes seguidas, você poderá ver valores muito diferentes porque os valores de IOPS e latências podem flutuar significativamente de um segundo para outro.

### <a name="BKMK_Updates"></a>Que nova funcionalidade foi adicionada no Windows Server 2016

No Windows Server 2016, os tipos de política de QoS de armazenamento foram renomeados.  O tipo de política **Várias instâncias** foi renomeado como **Dedicada** e **Instância única** foi renomeado como **Agregada**. O comportamento de gerenciamento de políticas Dedicadas também foi modificado. Os arquivos VHD/VHDX na mesma máquina virtual que tiver a mesma política **Dedicada** aplicada a eles não compartilharão alocações de E/S.  

Há dois novos recursos de QoS de armazenamento do Windows Server 2016:  

-   **Largura de banda máxima**  

    A QoS de armazenamento no Windows Server 2016 introduz a capacidade de especificar a largura de banda máxima que os fluxos atribuídos à política podem consumir.  O parâmetro ao especificá-lo nos cmdlets **StorageQosPolicy** é **MaximumIOBandwidth** e a saída é expressa em bytes por segundo.  
    Se ambos **MaximimIops** e **MaximumIOBandwidth** estiverem definidos em uma política, eles estarão em vigor e o primeiro deles a ser atingido pelos fluxos limitará a E/S dos fluxos.  

-   **A normalização de IOPS é configurável**  

    A QoS de armazenamento usa a normalização de IOPS.  O padrão é usar um tamanho de normalização de 8 K.  A QoS de armazenamento no Windows Server 2016 introduz a capacidade de especificar um tamanho diferente de normalização para o cluster de armazenamento.  Esse tamanho de normalização afeta todos os fluxos no cluster de armazenamento e entra em vigor imediatamente (dentro de alguns segundos) quando ele é alterado.  O mínimo é 1 KB e o máximo é 4 GB (não é recomendável definir mais de 4 MB, pois não é comum ter IOs maior que 4 MB).  

    Algo a ser considerado é que o mesmo padrão/rendimento de E/S aparece com números de IOPS diferentes na saída de QoS de armazenamento quando você altera a normalização de IOPS devido à alteração no cálculo de normalização.  Se você estiver comparando IOPS entre os clusters de armazenamento, talvez queira verificar qual o valor de normalização que cada um está usando porque isso afetará o IOPS normalizado relatado.    

#### <a name="example-1-creating-a-new-policy-and-viewing-the-maximum-bandwidth-on-the-storage-cluster"></a>Exemplo 1: criar uma nova política e exibir a largura de banda máxima no cluster de armazenamento  
No PowerShell, você pode especificar as unidades em que um número é expresso.  No exemplo a seguir, 10 MB são usados como o valor de largura de banda máxima.  A QoS de armazenamento converte e salva esse valor em bytes por segundo. Assim, 10 MB é convertido em 10485760 bytes por segundo.  

```PowerShell
PS C:\Windows\system32> New-StorageQosPolicy -Name HR_VMs -MaximumIops 1000 -MinimumIops 20 -MaximumIOBandwidth 10MB  

Name   MinimumIops MaximumIops MaximumIOBandwidth Status  
----   ----------- ----------- ------------------ ------  
HR_VMs 20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQosPolicy  

Name    MinimumIops MaximumIops MaximumIOBandwidth Status  
----    ----------- ----------- ------------------ ------  
Default 0           0           0                  Ok  
HR_VMs  20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQoSFlow | fL InitiatorName,FilePath,InitiatorIOPS,InitiatorLatency,InitiatorBandwidth  

InitiatorName      : testsQoS  
FilePath           : C:\ClusterStorage\Volume2\TESTSQOS\VIRTUAL HARD DISKS\TESTSQOS.VHDX  
InitiatorIOPS      : 5  
InitiatorLatency   : 1.5455  
InitiatorBandwidth : 37888  
```  

#### <a name="example-2-get-iops-normalization-settings-and-specify--a-new-value"></a>Exemplo 2: obter configurações de normalização de IOPS e especificar um novo valor  

O exemplo a seguir demonstra como obter as configurações de normalização de IOPS dos clusters de armazenamento (padrão de 8 KB) e, em seguida, configure-o como 32 KB e exiba-o novamente.  Observe que, neste exemplo, especifique "32 KB", porque o PowerShell permite especificar a unidade em vez de exigir a conversão em bytes.   A saída mostra o valor em bytes por segundo.  

```PowerShell
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
8192  

PS C:\Windows\system32> Set-StorageQosPolicyStore -IOPSNormalizationSize 32KB  
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
32768  
```    

## <a name="see-also"></a>Consulte também  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Réplica de armazenamento no Windows Server 2016](../storage-replica/storage-replica-overview.md)  
- [Espaços de Armazenamento Diretos no Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
