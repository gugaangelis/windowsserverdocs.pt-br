---
title: Visão geral de Espaços de Armazenamento Diretos
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/26/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: Uma visão geral de espaços de armazenamento diretos, um recurso do Windows Server que permite clusterizar servidores com armazenamento interno em uma solução de armazenamento definido pelo software.
ms.localizationpriority: medium
ms.openlocfilehash: d71e4fc404f760102a2b22f71bbeec680868e9b8
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262779"
---
# Visão geral de Espaços de Armazenamento Diretos

>Aplica-se a: Windows Server 2019, Windows Server 2016

O recurso Espaços de Armazenamento Diretos usa servidores padrão do setor com unidades conectadas localmente para criar um armazenamento definido pelo software altamente disponível e escalonável com custo menor do que o de matrizes de SAN ou NAS tradicionais. Sua arquitetura convergente ou hiperconvergente simplifica radicalmente a aquisição e a implantação, enquanto os recursos como caching, camadas de armazenamento e codificação de eliminação, juntamente com as mais recentes inovações de hardware, como a rede RDMA e unidades NVMe, oferecem desempenho e eficiência incomparáveis.

Espaços de armazenamento diretos está incluído no Windows Server 2019 Datacenter, Windows Server 2016 Datacenter e [Compilações do Windows Server Insider Preview](https://insider.windows.com/for-business-getting-started-server/). 

Para outros aplicativos de espaços de armazenamento, como clusters de SAS compartilhados e servidores autônomos, consulte [Visão geral de espaços de armazenamento](overview.md). Se você estiver procurando informações sobre como usar espaços de armazenamento em um computador Windows 10, consulte [Espaços de armazenamento no Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Entender</a></strong>
            <ul>
              <li>Visão geral (você está aqui)</li>
              <li><a href="understand-the-cache.md">Noções básicas sobre o cache</a></li>
              <li><a href="storage-spaces-fault-tolerance.md">Tolerância a falhas e eficiência de armazenamento</a></li>
              <li><a href="drive-symmetry-considerations.md">Considerações de simetria de unidade</a></li>
              <li><a href="understand-storage-resync.md">Entender e monitorar a ressincronização de armazenamento</a></li>
              <li><a href="understand-quorum.md">Quorum de cluster e pool de compreensão</a></li>
              <li><a href="cluster-sets.md">Conjuntos de cluster</a></li>
            </ul>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Planeje</a></strong>
            <ul>
              <li><a href="storage-spaces-direct-hardware-requirements.md">Requisitos de hardware</a></li>
              <li><a href="csv-cache.md">Usar o CSV na memória cache de leitura</li>
              <li><a href="choosing-drives.md">Escolha de unidades</a></li>
              <li><a href="plan-volumes.md">Planejamento de volumes</a></li>
              <li><a href="storage-spaces-direct-in-vm.md">Usar clusters de VM convidado</a></li>
              <li><a href="storage-spaces-direct-disaster-recovery.md">Recuperação de desastres</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Implantar</a></strong>
            <ul>
                    <li><a href="deploy-storage-spaces-direct.md">Implantar espaços de armazenamento diretos</a></li>
                    <li><a href="create-volumes.md">Criar volumes</a></li>
              <li><a href="nested-resiliency.md">Resiliência aninhada</a></li>
              <li><a href="../../failover-clustering/manage-cluster-quorum.md">Configurar quorum</a></li>
              <li><a href="upgrade-storage-spaces-direct-to-windows-server-2019.md">Atualizar um cluster de espaços de armazenamento diretos para o Windows Server 2019</a></li>
            </ul>
        </td>        
        <td style="padding: 5px; border: 0;">
            <strong>Gerencie</a></strong>
            <ul>
              <li><a href="../../manage/windows-admin-center/use/manage-hyper-converged.md">Gerenciar com o Windows Admin Center</a></li>
              <li><a href="add-nodes.md">Adicionar servidores ou unidades</a></li>
              <li><a href="maintain-servers.md">Colocar um servidor offline para manutenção</li>
              <li><a href="remove-servers.md">Remover servidores</a></li>
              <li><a href="resize-volumes.md">Estender volumes</a></li>
              <li><a href="../update-firmware.md">Atualizar firmware da unidade</a></li>
              <li><a href="performance-history.md">Histórico de desempenho</a></li>
              <li><a href="delimit-volume-allocation.md">Delimitar a alocação de volumes</a></li>
              <li><a href="configure-azure-monitor.md">Use o Monitor do Azure em um cluster com hiperconvergência</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
         <td style="padding: 5px; border: 0;">
            <strong>Solução de problemas</a></strong>
            <ul>
              <li><a href="storage-spaces-states.md">Solucionar problemas de integridade e estados operacionais</a></li>
              <li><a href="data-collection.md">Coletar dados de diagnóstico com espaços de armazenamento diretos</a></li>
            </ul>
         <td style="padding: 5px; border: 0;">
            <strong>Postagens de blog recentes</a></strong>
            <ul>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/">13.7 milhões IOPS com espaços de armazenamento diretos: o novo registro de setor de infraestrutura hiperconvergente</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/">Infraestrutura de hiperconvergência no Windows Server 2019 - o relógio de contagem regressiva começa agora!</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/">Cinco grandes avisos do Summit de servidor do Windows</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/">10.000 clusters de espaços de armazenamento diretos e contagem...</a></li>
            </ul>
</table>

## Vídeos

**Visão geral em vídeo rápido (5 minutos)**

<iframe src="https://www.youtube-nocookie.com/embed/raeUiNtMk0E" width="560" height="315" allowfullscreen></iframe>

**Espaços de armazenamento diretos no Microsoft Ignite 2018 (1 hora)**

[Assista no YouTube](https://www.youtube.com/watch?v=5kaUiW3qo30)

**Espaços de armazenamento diretos no Microsoft Ignite 2017 (1 hora)**

[Assista no YouTube](https://www.youtube.com/watch?v=YDr2sqNB-3c)

**Iniciar o evento na Microsoft Ignite 2016 (1 hora)**

[Assista no YouTube](https://www.youtube.com/watch?v=-LK2ViRGbWs)

## Principais benefícios

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Simplicidade.</b> Passe de servidores padrão do setor que executam o Windows Server 2016 para seu primeiro cluster de Espaços de Armazenamento Diretos em menos de 15 minutos. Para usuários do System Center, a implantação é apenas uma caixa de seleção.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/performance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Desempenho inigualável.</b> Se todos os flash ou híbrida, espaços de armazenamento Direct facilmente exceder <a href="https://blogs.technet.microsoft.com/filecab/2016/07/26/storage-iops-update-with-storage-spaces-direct/">150.000 misto 4K aleatório IOPS por servidor</a> com consistente e de baixa latência graças à sua arquitetura de hipervisor interno, seu interno cache de leitura/gravação e suporte para unidades de NVMe ponta montadas diretamente no barramento PCIe.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/fault-tolerance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Tolerância a falhas. </b> A resiliência interna manuseia falhas de unidade, servidor ou componente com disponibilidade contínua. Implantações maiores também podem ser configuradas para <a href="../../failover-clustering/fault-domains.md">a tolerância a falhas de chassis e do rack</a>. Quando o hardware falhar, basta trocá-lo; o software se repara sozinho, sem etapas de gerenciamento complicadas.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Eficiência de recursos.</b> A codificação de eliminação oferece eficiência de armazenamento até 2,4 x maior, com inovações exclusivas como Códigos de reconstrução local e camadas em tempo real ReFS para expandir esses ganhos a unidades de disco rígido e cargas de trabalho ativas/passivas mistas, tudo isso enquanto o consumo da CPU é minimizado para devolver recursos para onde eles são mais necessários – nas VMs.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Capacidade de gerenciamento.</b> Use <a href="../storage-qos/storage-qos-overview.md">Controles QoS de armazenamento</a> para manter VMs ocupadas excessivamente na verificação dos limites mínimo e máximo de IOPS por VM. O <a href="../../failover-clustering/health-service-overview.md">serviço de integridade</a> oferece monitoramento e alertas internos contínuos e novas APIs facilitam a coleta de métricas de capacidade e desempenho avançadas em todo o cluster.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Escalabilidade.</b> Tenha até 16 servidores e mais de 400 unidades para até 1 petabyte (1.000 terabytes) de armazenamento por cluster. Para escalar horizontalmente, basta adicionar unidades ou mais servidores. O recurso Espaços de Armazenamento Diretos integrará automaticamente novas unidades e começará a usá-las. O desempenho e a eficiência de armazenamento melhoram de maneira previsível grande escala.
        </td>
    </tr>
</table>

## Opções de implantação

O recurso Espaços de Armazenamento Diretos foi criado para duas opções de implantação distintas:

### Convergido

**Armazenamento e computação em clusters separados.** A opção de implantação convergente, também conhecida como "desagregada", dispõe um SoFS (Servidor de Arquivos de Escalabilidade Horizontal) em camadas sobre Espaços de Armazenamento Diretos para oferecer NAS por meio de compartilhamentos de arquivos SMB3. Isso permite o dimensionamento de computação/carga de trabalho independentemente do cluster de armazenamento, essencial para implantações em maior escala, como o IaaS (Infraestrutura como Serviço) Hyper-V para provedores de serviços e empresas.

![Os Espaços de Armazenamento Diretos servem para armazenamento usando o recurso Servidor de Arquivos de Escalabilidade Horizontal para VMs do Hyper-V em outro servidor ou cluster](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### Hiperconvergentes

**Um cluster para computação e armazenamento.** A opção de implantação hiperconvergente executa máquinas virtuais Hyper-V ou bancos de dados do SQL Server diretamente nos servidores que oferecem o armazenamento, armazenando seus arquivos em volumes locais. Isso acaba com a necessidade de configurar permissões e acesso ao servidor de arquivos de configuração e reduz os custos de hardware para empresas de pequeno a médio porte ou implantações do escritório remoto/filial. Consulte [implantar espaços de armazenamento diretos](deploy-storage-spaces-direct.md).

![Espaços de Armazenamento Diretos servem de armazenamento para VMs do Hyper-V no mesmo cluster](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## Como funciona

O recurso Espaços de Armazenamento Diretos é a evolução do Espaços de Armazenamento, introduzido pela primeira vez no Windows Server 2012. Ele usa muitos dos recursos que você conhece hoje no Windows Server, como Clustering de Failover, o sistema de arquivos CSV (Volume Compartilhado Clusterizado), protocolo SMB 3 e, claro, o Espaços de Armazenamento. Ele também introduz uma nova tecnologia, mais notavelmente Barramento de armazenamento de software.

Eis uma visão geral da pilha de Espaços de Armazenamento Diretos:

![Pilha de Espaços de Armazenamento Diretos](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**Hardware de rede.** O recurso Espaços de Armazenamento Diretos usa SMB3, incluindo o SMB Direct e o SMB Multichannel por Ethernet para comunicação entre servidores. É altamente recomendável ter mais de 10 GbE com RDMA (Acesso Remoto Direto à Memória), iWARP ou RoCE.

**Hardware de armazenamento.** De 2 a 16 servidores com unidades SATA, SAS ou NVMe conectadas localmente. Cada servidor deve ter pelo menos 2 unidades de estado sólido e pelo menos 4 unidades adicionais. Os dispositivos SATA e SAS devem ficar atrás de um HBA (adaptador de barramento do host) e expansor SAS. Recomendamos as plataformas meticulosamente projetadas e amplamente validadas dos nossos parceiros (em breve).

**Clustering de failover.** O recurso interno de clustering do Windows Server é usado para conectar os servidores.

**Barramento de Armazenamento de Software.** O Barramento de Armazenamento de Software é novo no recurso Espaços de Armazenamento Diretos. Ele abrange o cluster e estabelece uma malha de armazenamento definida pelo software por meio do qual todos os servidores podem ver todas as unidades locais uns dos outros. Você pode considerá-lo quanto for substituir o cabeamento caro e restritivo do Fibre Channel ou do SAS Compartilhado.

**Cache da Camada do Barramento de Armazenamento.** O Barramento de Armazenamento de Software associa dinamicamente as unidades mais rápidas presentes (por exemplo, SSD) às unidades mais lentas (por exemplo, HDDs) para fornecer o caching de leitura/gravação do lado do servidor que acelera a E/S e aumenta a taxa de transferência.

**Pool de armazenamento.** A coleção de unidades que forma a base do Espaços de Armazenamento é denominada pool de armazenamento. Ele é criado automaticamente e todas as unidades elegíveis são descobertas automaticamente e adicionadas a ele. É altamente recomendável que você use um pool por cluster, com as configurações padrão. Leia [Deep Dive into the Storage Pool](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/) para saber mais.

**Espaços de Armazenamento.** O recurso Espaços de Armazenamento oferece tolerância a falhas para "discos" virtuais usando o [espelhamento, codificação de eliminação ou ambos](storage-spaces-fault-tolerance.md). Você pode pensar nele como um RAID distribuído e definido pelo software que usa as unidades no pool. Em espaços de armazenamento direto, esses discos virtuais normalmente têm resiliência para duas unidades ou servidor falhas simultâneas (por exemplo, 3 vias espelhamento, com cada cópia de dados em um servidor diferente) embora chassi e tolerância a falhas de rack também está disponível.

**ReFS (Sistema de Arquivos Resiliente).** O ReFS é um sistema de arquivos premier desenvolvido especificamente para virtualização. Ele inclui acelerações significativas para operações de arquivo .vhdx como criação, expansão, mesclagem de ponto de verificação e somas de verificação internas para detectar e corrigir erros de bit. Ele também apresenta camadas em tempo real que rotacionam dados entre camadas de armazenamento "quentes" e "frias" em tempo real com base na utilização.

**Volume Compartilhado Clusterizado.** O sistema de arquivos CSV unifica todos os volumes ReFS em um único namespace acessível por meio de qualquer servidor para que cada servidor e cada volume pareçam e ajam como são montados localmente.

**Servidor de Arquivos de Escalabilidade Horizontal.** Essa camada final é necessária somente em implantações convergentes. Ela oferece acesso a arquivo remoto usando o protocolo de acesso SMB3 aos clientes, como outro cluster que executa o Hyper-V pela rede, transformando efetivamente Espaços de Armazenamento Diretos em NAS.

## Histórias de clientes

Há [mais de 10.000 clusters](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/) em todo o mundo em execução espaços de armazenamento diretos. As organizações de todos os tamanhos, desde pequenas empresas implantar apenas dois nós, para grandes empresas e governos Implantando centenas de nós, dependem de espaços de armazenamento diretos para seus aplicativos críticos e infraestrutura.

Visite [Microsoft.com/HCI](https://www.microsoft.com/hci) para ler suas histórias:

[![Geliminar logotipos de cliente](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## Ferramentas de gerenciamento

As seguintes ferramentas podem ser usadas para gerenciar e/ou monitorar espaços de armazenamento diretos:

| Nome | Linha de comando ou gráfica? | Pago ou incluído? |
|-----------------|----------------------------|-------------------|
| [WindowsAdmin Center](../../manage/windows-admin-center/overview.md)     | Gráfica    | Incluído |
| Gerenciador de Cluster de Failover do Gerenciador do servidor &                                 | Gráfica    | Incluído |
| Windows PowerShell                                                        | Linha de comando | Incluído |
| [O System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) & [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | Gráfica    | Pagos     |

## Introdução

Experimente Espaços de Armazenamento Diretos [no Microsoft Azure](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/), ou baixe uma cópia de avaliação de 180 dias licenciados do Windows Server em [Avaliações do Windows Server](https://go.microsoft.com/fwlink/?linkid=842602).

## Consulte também

-   [Tolerância a falhas e eficiência de armazenamento](storage-spaces-fault-tolerance.md)
-   [Réplica de armazenamento](../storage-replica/storage-replica-overview.md)
-   [Armazenamento no blog da Microsoft](https://blogs.technet.microsoft.com/filecab/)
-   [Taxa de transferência de Espaços de Armazenamento Diretos com iWARP](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp) (blog do TechNet)
-   [Novidades em Clustering de Failover no Windows Server](../../failover-clustering/whats-new-in-failover-clustering.md)  
-   [Qualidade do serviço de armazenamento](../storage-qos/storage-qos-overview.md)
-   [Suporte do Windows IT Pro](https://www.microsoft.com/itpro/windows/support)
