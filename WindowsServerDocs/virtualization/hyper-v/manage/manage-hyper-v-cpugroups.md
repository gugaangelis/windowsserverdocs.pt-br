---
title: Controles de recurso de máquina virtual
description: Como usar grupos de CPU de VM
author: allenma
ms.prod: windows-server
ms.date: 06/18/2018
ms.topic: article
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: dc26dbe6bda9dc4f4f9b2bd99b3b8400555651ac
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077183"
---
# <a name="virtual-machine-resource-controls"></a>Controles de recurso de máquina virtual

> Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Este artigo descreve os controles de isolamento e recursos do Hyper-V para máquinas virtuais.  Esses recursos, que vamos nos referir como grupos de CPU de máquina virtual, ou apenas "grupos de CPU", foram introduzidos no Windows Server 2016.  Os grupos de CPU permitem que os administradores do Hyper-V gerenciem e aloquem melhor os recursos de CPU do host em máquinas virtuais convidadas.  Usando grupos de CPU, os administradores do Hyper-V podem:

* Crie grupos de máquinas virtuais, com cada grupo com alocações diferentes dos recursos totais da CPU do host de virtualização, compartilhados em todo o grupo. Isso permite que o administrador do host implemente classes de serviço para diferentes tipos de VMs.

* Defina limites de recursos de CPU para grupos específicos. Essa "extremidade do grupo" define o limite superior para os recursos de CPU do host que todo o grupo pode consumir, impondo efetivamente a classe de serviço desejada para esse grupo.

* Restringir um grupo de CPU a ser executado somente em um conjunto específico de processadores do sistema host. Isso pode ser usado para isolar as VMs que pertencem a diferentes grupos de CPU uns dos outros.

## <a name="managing-cpu-groups"></a>Gerenciando grupos de CPU

Os grupos de CPU são gerenciados por meio do serviço de computação do host Hyper-V ou HCS. Uma grande descrição do HCS, seu Genesis, links para as APIs do HCS e muito mais está disponível no blog da equipe de virtualização da Microsoft na postagem [que apresenta o HCS (serviço de computação do host)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/).

>[!NOTE]
>Somente o HCS pode ser usado para criar e gerenciar grupos de CPU; o miniaplicativo do Gerenciador do Hyper-V, as interfaces de gerenciamento do WMI e do PowerShell não dão suporte a grupos de CPU.

A Microsoft fornece um utilitário de linha de comando, cpugroups.exe, no [centro de download da Microsoft](https://go.microsoft.com/fwlink/?linkid=865968) que usa a interface HCS para gerenciar grupos de CPU.  Esse utilitário também pode exibir a topologia de CPU de um host.

## <a name="how-cpu-groups-work"></a>Como funcionam os grupos de CPU

A alocação de recursos de computação do host em grupos de CPU é imposta pelo hipervisor do Hyper-V, usando um limite de grupo de CPU calculado. O limite do grupo de CPU é uma fração da capacidade total da CPU para um grupo de CPU. O valor da extremidade do grupo depende da classe do grupo ou do nível de prioridade atribuído. O limite do grupo computado pode ser considerado como "um número de LP do tempo de CPU". Esse orçamento de grupo é compartilhado, portanto, se apenas uma única VM estivesse ativa, ela poderia usar a alocação de CPU do grupo inteiro para si mesma.

O limite do grupo de CPU é calculado como G = *n* x *C*, em que:

- *G* é a quantidade de host LP que gostaríamos de atribuir ao grupo
- *n* é o número total de processadores lógicos (LPs) no grupo
- *C* é a alocação de CPU máxima — ou seja, a classe de serviço desejada para o grupo, expressa como uma porcentagem da capacidade total de computação do sistema

Por exemplo, considere um grupo de CPU configurado com 4 processadores lógicos (LPs) e um limite de 50%.

- G = n * C
- G = 4 * 50%
- G = 2 LP de tempo de CPU para o grupo inteiro

Neste exemplo, o grupo de CPU G é alocado 2 LP de tempo de CPU.

Observe que o limite do grupo se aplica independentemente do número de máquinas virtuais ou processadores virtuais associados ao grupo e, independentemente do estado (por exemplo, desligamento ou inicialização) das máquinas virtuais atribuídas ao grupo de CPU. Portanto, cada VM associada ao mesmo grupo de CPU receberá uma fração da alocação total de CPU do grupo, e isso será alterado com o número de VMs associadas ao grupo de CPU. Portanto, à medida que as VMs são associadas ou desassociadas a VMs de um grupo de CPU, a extremidade geral do grupo de CPU deve ser reajustada e definida para manter o limite resultante por VM desejado. A camada de software de gerenciamento de virtualização ou administrador de host de VM é responsável por gerenciar Caps de grupo conforme necessário para atingir a alocação de recursos de CPU por VM desejada.

## <a name="example-classes-of-service"></a>Exemplo de classes de serviço

Vejamos alguns exemplos simples. Para começar, suponha que o administrador do host Hyper-V queira dar suporte a duas camadas de serviço para VMs convidadas:

1. Uma camada "C" de low-end. Forneceremos a essa camada 10% dos recursos de computação do host inteiro.

1. Uma camada intermediária "B". Essa camada é alocada 50% dos recursos de computação do host inteiro.

Neste ponto em nosso exemplo, vamos declarar que nenhum outro controle de recurso de CPU está em uso, como limites de VM individuais, pesos e reservas.
No entanto, os limites de VM individuais são importantes, como veremos um pouco mais tarde.

Para simplificar, vamos supor que cada VM tenha 1 VP e que nosso host tenha 8 LPs. Começaremos com um host vazio.

Para criar a camada "B", o host adminstartor define a Cap do grupo como 50%:

- G = n * C
- G = 8 * 50%
- G = 4 LP de tempo de CPU para o grupo inteiro

O administrador de host adiciona uma única VM de camada "B".
Neste ponto, nossa VM de camada "B" pode usar no máximo 50% da CPU do host ou o equivalente a 4 LPs em nosso sistema de exemplo.

Agora, o administrador adiciona uma segunda VM de "camada B". A alocação do grupo de CPU — é dividida uniformemente entre todas as VMs. Temos um total de 2 VMs no grupo B, portanto, cada VM agora Obtém a metade do total do grupo de 50%, 25% a cada ou o equivalente a 2 LPs de tempo de computação.

## <a name="setting-cpu-caps-on-individual-vms"></a>Definindo limites de CPU em VMs individuais

Além do limite do grupo, cada VM também pode ter uma "extremidade da VM" individual. Os controles de recurso de CPU por VM, incluindo um limite de CPU, peso e reserva, fazem parte do Hyper-V desde sua introdução.
Quando combinado com um limite de grupo, uma extremidade de VM especifica a quantidade máxima de CPU que cada VP pode obter, mesmo que o grupo tenha recursos de CPU disponíveis.

Por exemplo, o administrador de host pode querer posicionar uma Cap de 10% de VM em VMs "C".
Dessa forma, mesmo que a maioria das "C" VPSs estejam ociosas, cada VP nunca poderá obter mais de 10%.
Sem uma Cap de VM, as VMs "C" podem obter oportunamente o desempenho além dos níveis permitidos pela sua camada.

## <a name="isolating-vm-groups-to-specific-host-processors"></a>Isolando grupos de VM para processadores de host específicos

Os administradores de host do Hyper-V também podem querer a capacidade de dedicar recursos de computação a uma VM.
Por exemplo, imagine que o administrador queria oferecer uma VM "A" Premium que tenha um limite de classe de 100%.
Essas VMs Premium também exigem a menor latência de agendamento e a variação possível; ou seja, eles não podem ser desagendados por nenhuma outra VM.
Para obter essa separação, um grupo de CPU também pode ser configurado com um mapeamento de afinidade de LP específico.

Por exemplo, para se ajustar a uma VM "A" no host em nosso exemplo, o administrador criaria um novo grupo de CPU e definirá a afinidade de processador do grupo para um subconjunto do LPs do host.
Os grupos B e C seriam relacionados para a LPs restante.
O administrador pode criar uma única VM no grupo A, que teria acesso exclusivo a todos os LPs no grupo A, enquanto os grupos de camadas B e C presumidamente inferiores compartilharão o restante do LPs.

## <a name="segregating-root-vps-from-guest-vps"></a>VPSs raiz segregando do convidado VPSs

Por padrão, o Hyper-V criará um vice-presidente raiz em cada LP físico subjacente.
Esses Vpsss raiz são estritamente mapeados 1:1 com o sistema LPs e não são migrados — ou seja, cada VP raiz sempre será executado no mesmo LP físico.
O convidado VPSs pode ser executado em qualquer LP disponível e compartilhará a execução com o VPSs raiz.

No entanto, pode ser desejável separar completamente a atividade de VP raiz do convidado VPSs.
Considere nosso exemplo acima, no qual implementamos uma VM de camada Premium "A".
Para garantir que o VPSs de uma VM "A" tenha a menor latência possível e a "tremulação" ou a variação de agendamento, gostaríamos de executá-las em um conjunto dedicado de LPs e garantir que a raiz não seja executada nesses LPs.

Isso pode ser feito usando uma combinação da configuração "minroot", que limita a execução da partição do sistema operacional do host em um subconjunto do total de processadores lógicos do sistema, juntamente com um ou mais grupos de CPU relacionados.

O host de virtualização pode ser configurado para restringir a partição de host a LPs específicas, com um ou mais grupos de CPU relacionados para o restante de LPs.
Dessa maneira, as partições raiz e de convidado podem ser executadas em recursos de CPU dedicados e completamente isoladas, sem compartilhamento de CPU.

Para obter mais informações sobre a configuração "minroot", consulte [Gerenciamento de recursos da CPU do host Hyper-V](./manage-hyper-v-minroot-2016.md).

## <a name="using-the-cpugroups-tool"></a>Usando a ferramenta CpuGroups

Vejamos alguns exemplos de como usar a ferramenta CpuGroups.

>[!NOTE]
>Os parâmetros de linha de comando para a ferramenta CpuGroups são passados usando apenas espaços como delimitadores. Nenhum caractere '/' ou '-' deve prosseguir com a opção de linha de comando desejada.

### <a name="discovering-the-cpu-topology"></a>Descobrindo a topologia da CPU

A execução de CpuGroups com o GetCpuTopology retorna informações sobre o sistema atual, como mostrado abaixo, incluindo o índice de LP, o nó NUMA ao qual o LP pertence, o pacote e as IDs de núcleo e o índice de VP raiz.

O exemplo a seguir mostra um sistema com dois soquetes de CPU e nós NUMA, um total de 32 LPs e vários threads habilitados e configurados para habilitar Minroot com 8 VPSs raiz, 4 de cada nó NUMA.
Os LPs que têm VPSs raiz têm um RootVpIndex >= 0; LPs com um RootVpIndex de-1 não estão disponíveis para a partição raiz, mas ainda são gerenciados pelo hipervisor e serão executados VPSs de convidado conforme permitido por outras definições de configuração.

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

Aqui, listaremos todos os grupos de CPU no host atual, seu GroupId, o limite de CPU do grupo e as índicos de LPs atribuídas a esse grupo.

Observe que os valores de limite de CPU válidos estão no intervalo [0, 65536], e esses valores expressam o limite do grupo em porcentagem (por exemplo, 32768 = 50%).

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>Exemplo 3 – imprimir um único grupo de CPU

Neste exemplo, consultaremos um único grupo de CPU usando GroupId como um filtro.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>Exemplo 4 – criar um novo grupo de CPU

Aqui, criaremos um novo grupo de CPU, especificando a ID do grupo e o conjunto de LPs para atribuir ao grupo.

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

Agora, exiba nosso grupo recém-adicionado.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>Exemplo 5 – definir o limite do grupo de CPU como 50%

Aqui, vamos definir o limite do grupo de CPU como 50%.

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

Agora, vamos confirmar nossa configuração exibindo o grupo que acabamos de atualizar.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>Exemplo 6 – imprimir IDs de grupo de CPU para todas as VMs no host

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

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>Exemplo 7 – desassociar uma VM do grupo de CPU

Para remover uma VM de um grupo de CPU, defina para CpuGroupId da VM como o GUID nulo. Isso desassociará a VM do grupo de CPU.

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

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>Exemplo 8 – associar uma VM a um grupo de CPU existente

Aqui, adicionaremos uma VM a um grupo de CPU existente.
Observe que a VM não deve estar associada a nenhum grupo de CPU existente ou a definição da ID do grupo de CPU falhará.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

Agora, confirme se a VM G1 está no grupo de CPU desejado.

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

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>Exemplo 9 – imprimir todas as VMs agrupadas por ID de grupo de CPU

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

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>Exemplo 11 – tentando excluir um grupo de CPU não vazio

Somente grupos de CPU vazios, ou seja, grupos de CPU sem VMs associadas, podem ser excluídos.
A tentativa de excluir um grupo de CPU não vazio falhará.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>Exemplo 12 – desassociar a única VM de um grupo de CPU e excluir o grupo

Neste exemplo, usaremos vários comandos para examinar um grupo de CPU, remover a única VM pertencente a esse grupo e, em seguida, excluir o grupo.

Primeiro, vamos enumerar as VMs em nosso grupo.

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

Vemos que apenas uma única VM, chamada G1, pertence a esse grupo.
Vamos remover a VM G1 de nosso grupo definindo a ID do grupo da VM como NULL.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

E verificar nossa alteração...

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

Agora que o grupo está vazio, podemos excluí-lo com segurança.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

E confirme que nosso grupo foi eliminado.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>Exemplo 13 – associar uma VM de volta ao seu grupo de CPU original

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