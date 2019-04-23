---
title: Controles de recursos de máquina virtual
description: Como usar grupos de CPU de VM
keywords: windows 10, hyper-v
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: 7c4ddf3e5d2ff58eef844c50960327c27a3e0a3d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854757"
---
>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="virtual-machine-resource-controls"></a>Controles de recursos de máquina virtual

Este artigo descreve os controles de recurso e o isolamento do Hyper-V para máquinas virtuais.  Esses recursos, que chamaremos como grupos de CPU da máquina Virtual, ou apenas "grupos de CPU", foram introduzidos no Windows Server 2016.  Grupos de CPU permitem que os administradores do Hyper-V para melhor gerenciar e alocam recursos de CPU do host entre máquinas virtuais convidadas.  Usando grupos de CPU, os administradores do Hyper-V podem:

* Crie grupos de máquinas virtuais, com cada grupo tendo alocações diferentes total do host de virtualização recursos de CPU, compartilhados em todo o grupo. Isso permite que o administrador do host implementar as classes de serviço para diferentes tipos de máquinas virtuais.

* Definir limites de recursos de CPU para grupos específicos. Esse limite"grupo" define o limite superior para o host recursos da CPU que pode consumir todo o grupo, efetivamente, impondo a classe desejada de serviço para esse grupo.

* Restringir um grupo de CPU para executar apenas em um conjunto específico de processadores do sistema host. Isso pode ser usado para isolar as máquinas virtuais que pertencem a grupos de CPU diferentes uns dos outros.

## <a name="managing-cpu-groups"></a>Gerenciando grupos de CPU

Grupos de CPU são gerenciados por meio do serviço de computação do Host do Hyper-V ou HCS. Uma descrição excelente do HCS, seu gênese, links para as APIs de HCS e muito mais está disponível no blog da equipe Microsoft Virtualization no lançamento [apresentando o serviço de computação do Host (HCS)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/).

>[!NOTE] 
>Somente o HCS pode ser usado para criar e gerenciar grupos de CPU; o miniaplicativo do Gerenciador do Hyper-V, as interfaces de gerenciamento de WMI e do PowerShell não dão suporte a grupos de CPU.

A Microsoft fornece uma linha de comando do utilitário, cpugroups.exe, sobre o [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=865968) que usa a interface HCS para gerenciar grupos de CPU.  Esse utilitário também pode exibir a topologia de CPU de um host.

## <a name="how-cpu-groups-work"></a>Como funcionam os grupos de CPU

Alocação de recursos de computação do host entre grupos de CPU é imposta pelo hipervisor Hyper-V, usando um limite de grupo de CPU computado. O limite de grupo de CPU é uma fração da capacidade total da CPU para um grupo de CPU. O valor do limite de grupo depende da classe de grupo, ou o nível de prioridade atribuído. O limite de grupo computada pode ser pensado como "um número de LP's que vale a pena de tempo de CPU". Este orçamento de grupo for compartilhado, se apenas uma única VM estavam ativos, ele poderia usar alocação de CPU do grupo inteiro para si mesmo.

O limite de grupo da CPU é calculado como G = *n* x *C*, onde:

    *G* is the amount of host LP we'd like to assign to the group
    *n* is the total number of logical processors (LPs) in the group
    *C* is the maximum CPU allocation — that is, the class of service desired for the group, expressed as a percentage of the system’s total compute capacity

Por exemplo, considere um grupo de CPU configurado com 4 processadores lógicos (LPs) e um limite de 50%.

    G = n * C
    G = 4 * 50%
    G = 2 LP's worth of CPU time for the entire group

Neste exemplo, o grupo de CPU G é alocado 2 LP's que vale a pena de tempo de CPU.  

Observe que o limite de grupo se aplica independentemente do número de máquinas virtuais ou vinculados ao grupo de processadores virtuais e independentemente do estado (por exemplo, desligamento ou iniciada) das máquinas virtuais atribuídas ao grupo de CPU. Portanto, cada VM associada ao mesmo grupo de CPU receberão uma fração da alocação de CPU total do grupo, e isso será alterado com o número de VMs associada ao grupo de CPU. Portanto, como as VMs estão associadas ou não associados a VMs de um grupo de CPU, o limite de grupo geral da CPU deve ser reajustado e definido para manter o limite por VM resultante desejado. O gerenciamento da VM host virtualização ou o administrador de camada de software é responsável por gerenciar caps grupo conforme necessário para alcançar a alocação de recursos de CPU por VM desejada.

## <a name="example-classes-of-service"></a>Classes de exemplo de serviço

Vamos examinar alguns exemplos simples. Para começar, suponha que o administrador do host do Hyper-V gostaria de dar suporte a duas camadas de serviço para VMs convidadas:

1. Uma camada de "C" low-end. Vamos dar essa camada de 10% dos recursos de computação do host inteiro.

1. Uma camada intermediária "B". Essa camada é alocada 50% dos recursos de computação do host inteiro.

Neste ponto em nosso exemplo, podemos vai assert que nenhum outro controle de recursos de CPU estão em uso, como limites VM individuais, pesos e reserva.
No entanto, os limites VM individuais são importantes, como veremos mais adiante.

Para simplificar, vamos supor que cada VM tem 1 VP e que nosso host tenha 8 LPs. Vamos começar com um host vazio.

Para criar a camada de "B", o host adminstartor define o limite de grupo para 50%:

    G = n * C
    G = 8 * 50%
    G = 4 LP's worth of CPU time for the entire group

O administrador do host adiciona uma única VM de camada de "B".
Neste ponto, nossa camada de "B" VM pode usar no máximo 50% que vale a pena de CPU do host ou o equivalente a 4 LPs em nosso sistema de exemplo.

Agora, o administrador adiciona um segundo "camada B" VM. Alocação de CPU do grupo — é dividido igualmente entre todas as VMs. Nós temos um total de 2 VMs no grupo B, portanto, cada VM agora obtém metade do total do grupo B de 50%, 25% cada ou o equivalente a 2 LPs que vale a pena de tempo de computação.

## <a name="setting-cpu-caps-on-individual-vms"></a>Definição de limites de CPU em VMs individuais

O limite de grupo, além de cada VM também pode ter uma individuais "cap VM". Controles de recursos de CPU por VM, incluindo um limite de CPU, peso e reserva, tem sido uma parte do Hyper-V desde sua introdução.
Quando combinado com um limite de grupo, um limite VM Especifica a quantidade máxima de CPU que pode obter cada vice-Presidente, mesmo se o grupo tem recursos de CPU disponíveis.

Por exemplo, o administrador do host talvez queira colocar um limite VM de 10% em VMs de "C".
Dessa forma, mesmo que a maioria dos vice-presidentes de "C" estão ociosos, cada VP nunca conseguíamos ter mais de 10%.
Sem um limite VM, VMs "C" oportunamente poderia obter desempenho além dos níveis permitido pela sua camada.

## <a name="isolating-vm-groups-to-specific-host-processors"></a>Isolar os grupos VM para processadores de Host específico

Os administradores de host Hyper-V podem ser conveniente a capacidade de dedicar recursos de computação para uma VM.
Por exemplo, imagine que o administrador desejava oferecer um premium "A" máquina virtual que tem um limite de classe de 100%.
Essas VMs premium também exigem a latência mais baixa de agendamento e variação possíveis; ou seja, eles podem não ser desprogramados por qualquer outra VM.
Para atingir essa separação, um grupo de CPU também pode ser configurado com um mapeamento de afinidade de LP específico.

Por exemplo, de acordo com uma VM "A" no host em nosso exemplo, o administrador seria criar um novo grupo de CPU e definir a afinidade de processador do grupo a um subconjunto de LPs do host.
Os grupos B e C teriam afinidade com os LPs restantes.
O administrador pode criar uma única VM no grupo A, que teria acesso exclusivo ao LPs todos no grupo A, enquanto os grupos de camada inferiores supostamente B e C compartilhariam os LPs restantes.

## <a name="segregating-root-vps-from-guest-vps"></a>Separando os vice-presidentes da raiz do convidado vice-presidentes

Por padrão, o Hyper-V será criar uma raiz VP em cada LP físico subjacente.
Esses vice-presidentes raiz são estritamente mapeados 1:1 com o sistema LPs e não migrar — ou seja, cada raiz VP sempre será executado sobre o LP físico mesmo.
Vice-presidentes convidado poderá ser executado em qualquer LP disponível e compartilhará a execução com raiz vice-presidentes.

No entanto, ele pode ser desejável para raiz completamente separada atividade vice-Presidente de vice-presidentes de convidado.
Considere o nosso exemplo acima no qual podemos implementar uma VM de camada de "A" premium.
Para garantir que os vice-presidentes "A" da nossa VM têm a menor latência possível e o "variação" ou a variação de agendamento, gostaríamos de executá-los em um conjunto dedicado de LPs e certifique-se de que a raiz não é executado nesses LPs.

Isso pode ser feito usando uma combinação da configuração "minroot", que limita o host de partição do sistema operacional em execução em um subconjunto dos processadores lógicos total do sistema, junto com uma ou mais afinidade com grupos de CPU.

O host de virtualização pode ser configurado para restringir a partição de host para LPs específicos, com um ou mais grupos de CPU afinidade com os LPs restantes.
Dessa forma, as partições raiz e o convidado podem ser executado em recursos dedicados de CPU e completamente isolado, sem compartilhamento de CPU.

Para obter mais informações sobre a configuração de "minroot", consulte [gerenciamento de recursos de CPU de Host do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016).

## <a name="using-the-cpugroups-tool"></a>Usando a ferramenta CpuGroups

Vamos examinar alguns exemplos de como usar a ferramenta CpuGroups.

>[!NOTE] 
>Parâmetros de linha de comando para a ferramenta CpuGroups são passados usando apenas espaços como delimitadores. Não '/' ou '-' caracteres devem continuar a opção de linha de comando desejado.

### <a name="discovering-the-cpu-topology"></a>Descobrir a topologia de CPU

A execução de CpuGroups com o GetCpuTopology retorna informações sobre o sistema atual, conforme mostrado abaixo, incluindo o índice de LP, o nó ao qual o LP pertence, o pacote e as IDs do Core e o índice de vice-Presidente de raiz.

O exemplo a seguir mostra um sistema com 2 soquetes de CPU e nós NUMA, um total de 32 LPs e várias threading habilitado e configuraram para permitir Minroot com 8 raiz vice-presidentes, 4 de cada nó NUMA.
Os LPs que têm vice-presidentes raiz tem um RootVpIndex > = 0; LPs com um RootVpIndex de -1 não estão disponíveis para a partição raiz, mas ainda são gerenciadas pelo hipervisor e serão executado pelo VP de convidado, conforme permitido por outras definições de configuração.

```console
C:\vm\tools>CpuGroups.exe GetCpuTopology

LpIndex NodeNumber PackageId CoreId RootVpIndex
------- ---------- --------- ------ -----------
      0          0         0      0           0
      1          0         0      0           1
      2          0         0      1           2
      3          0         0      1           3
      4          0         0      2          -1
      5          0         0      2          -1
      6          0         0      3          -1
      7          0         0      3          -1
      8          0         0      4          -1
      9          0         0      4          -1
     10          0         0      5          -1
     11          0         0      5          -1
     12          0         0      6          -1
     13          0         0      6          -1
     14          0         0      7          -1
     15          0         0      7          -1
     16          1         1     16           4
     17          1         1     16           5
     18          1         1     17           6
     19          1         1     17           7
     20          1         1     18          -1
     21          1         1     18          -1
     22          1         1     19          -1
     23          1         1     19          -1
     24          1         1     20          -1
     25          1         1     20          -1
     26          1         1     21          -1
     27          1         1     21          -1
     28          1         1     22          -1
     29          1         1     22          -1
     30          1         1     23          -1
     31          1         1     23          -1
```

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>Exemplo 2 – imprimir todos os grupos de CPU no host

Aqui, podemos irá listar todos os grupos de CPU no host atual, seu GroupId, limite de CPU do grupo e os índices de LPs atribuídos a esse grupo.

Observe que os valores válidos de limite de CPU estão no intervalo [0, 65536], e esses valores express o limite de grupo em porcentagem (por exemplo, 32768 = 50%).

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>Exemplo 3 – imprimir um único grupo de CPU

Neste exemplo, podemos irá consultar um único grupo de CPU, usando a ID do grupo como um filtro.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>Exemplo 4 – criar um novo grupo de CPU

Aqui, vamos criar um novo grupo de CPU, especificando a ID do grupo e o conjunto de LPs para atribuir ao grupo.

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

Agora exibem nosso grupo recém-adicionado.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>Exemplo 5 – definir o limite de grupo de CPU a 50%

Aqui, vamos definir o limite de grupo de CPU a 50%.

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

Agora vamos confirmar nossa configuração, exibindo o grupo que acabou de atualizar.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>Exemplo 6 – ids de grupo de CPU de impressão para todas as VMs no host

```console
C:\vm\tools>CpuGroups.exe GetVmGroup

VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36ab08cb-3a76-4b38-992e-000000000002
```

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>Exemplo 7 – desassociar uma VM a partir do grupo de CPU

Para remover uma máquina virtual de um grupo de CPU, defina CpuGroupId da VM para o GUID de nulo. Isso desassocia a VM a partir do grupo de CPU.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000

C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>Exemplo 8 – associar uma VM a um grupo existente de CPU

Aqui, vamos adicionar uma VM a um grupo existente de CPU.
Observe que a VM não deve ser associada a nenhum grupo de CPU existente, ou haverá falha na id do grupo de configuração da CPU.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

Agora, confirme que o G1 VM está no grupo da CPU desejado.

```console
C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36AB08CB-3A76-4B38-992E-000000000001
```

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>Exemplo 9 – imprimir todas as máquinas virtuais, agrupadas por id do grupo de CPU

```console
C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId                           VmName                                 VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36ab08cb-3a76-4b38-992e-000000000003     P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004     P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>Exemplo 10 – imprimir todas as VMs para um único grupo de CPU

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>Exemplo 11 – a tentativa de excluir um grupo de CPU não vazio

Deixar em branco somente grupos de CPU — ou seja, os grupos de CPU sem nenhum associado VMs — pode ser excluído.
Tentativa de excluir um grupo de CPU não-vazia falhará.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>Exemplo 12 – desassociar a única VM de um grupo de CPU e exclua o grupo

Neste exemplo, podemos vai usar vários comandos para examinar um grupo de CPU, remova a VM única que pertencem a esse grupo e exclua o grupo.

Primeiro, vamos enumerar as VMs em nosso grupo.

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

Podemos ver que somente uma única VM, nomeada G1, pertence a esse grupo.
Vamos remover a VM G1 do nosso grupo definindo a ID do grupo da VM como NULL.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

E verificar nossa mudança...

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

Agora que o grupo estiver vazio, podemos pode excluí-lo com segurança.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

E confirme se que nosso grupo não existe mais.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>Exemplo 13 – associar uma VM volta a seu grupo de CPU original

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000002

C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId VmName VmId
------------------------------------ -------------------------------- ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002 G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002 G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36AB08CB-3A76-4B38-992E-000000000002 G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000003 P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004 P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```
