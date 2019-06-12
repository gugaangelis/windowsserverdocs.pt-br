---
title: Data center definido por software do Windows Server
description: Visão geral do Windows Server SDDC
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: SDDC
ms.tgt_pltfrm: na
ms.topic: get-started article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/04/2019
ms.localizationpriority: medium
ms.openlocfilehash: 02b425d81eda22bf7608b44bef0212cf4462e42f
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501686"
---
# <a name="windows-server-software-defined-datacenter"></a>Datacenter definido por software do Windows Server

>Aplica-se a: Windows Server 2019, Windows Server 2016

![](media/sddc/heading.png)

## <a name="what-is-windows-server-software-defined-datacenter"></a>O que é o datacenter definidas por software do Windows Server?

Definida pelo software SDDC (datacenter) é um termo comum no mercado que geralmente se refere a um data center em que toda a infraestrutura é virtualizado. A virtualização é a chave, e significa que o hardware e o software no datacenter expandem além de uma proporção individual tradicional. Com um hardware de emulação de hipervisor de software, é possível abstrair sistemas operacionais e aplicativos do hardware físico e multiplicar para formar grupos de recursos elásticos de processadores, memória, E/S e redes.
 
Implementação do SDDC da Microsoft consiste nas tecnologias do Windows Server destacadas neste artigo. Começa com o hipervisor do Hyper-V que fornece a plataforma de virtualização no qual a rede e o armazenamento são criados. As tecnologias de segurança, desenvolvidas para desafios específicos da infraestrutura virtualizada, reduzem as ameaças internas e externas. Com o PowerShell integrado ao Windows Server e a adição do [System Center](https://docs.microsoft.com/system-center/) e/ou [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview), você pode programar e automatizar o provisionamento, a implantação, a configuração e o gerenciamento.

As tecnologias integradas ao Windows Server e ao System Center são os blocos de construção principais da experiência do Windows Server SDDC. Mas, mesmo que seja uma plataforma virtualizada, ela ainda requer o hardware adequado. Parceiros da Microsoft participando a **Windows Server Software-Defined (WSSD) de soluções** e o **soluções do Azure Stack HCI** programas podem ajudar sua empresa adquirir o hardware correto e colocá-lo e em execução no dia zero.

![](media/sddc/video.png) **[Assista a um vídeo para saber mais sobre o Microsoft do SDDC](https://mva.microsoft.com/en-US/training-courses/whats-new-in-windows-server-2016-16457?l=YcsJR6sXC_1006218965)**

![](media/sddc/poster-ico.png) **[Baixar um arquivo. PDF do pôster tamanho desta página](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs/media/sddc/sddc_poster_0801417_ANSI-E.pdf)**

![](media/sddc/spacer1.png)<a href="https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs//media/sddc/sddc_poster_0801417_ANSI-E.pdf"><img src="media/sddc/poster.png"></a>

## <a name="azure-stack-hci-solutions"></a>Soluções de pilha HCI do Azure

Criar seu data center definido pelo software do Windows Server na infraestrutura de hardware certo é uma primeira etapa crucial para o sucesso. É por isso que fizemos uma parceria com parceiros de 15 para criar designs SDDC validados pelo Microsoft e práticas recomendadas para implantação.

Parceiros da Microsoft oferecem uma variedade de soluções que funcionam com o servidor de janela 2019 por meio do programa HCI de pilha do Azure e o Windows Server 2016 por meio do programa do Windows Server definida pelo software (WSSD) para oferecer alto desempenho, hiperconvergente, rede e armazenamento infraestrutura. As soluções com hiperconvergência unem a computação, o armazenamento e a rede em servidores padrão do setor e os componentes para melhorar a inteligência e o controle do datacenter.

![](media/sddc/learn.png) **[Saiba mais sobre as soluções do Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci)**

![](media/sddc/learn.png) **[Saiba mais sobre as soluções de WSSD](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)**

## <a name="windows-server-virtualized-technologies"></a>Tecnologias virtualizadas do Windows Server ##

O restante deste tópico lista as tecnologias do Windows Server SDDC e fornece links para a documentação de cada um. As tecnologias são listadas na tabela abaixo.

![](media/sddc/table.png)

![](media/sddc/virtualize.png)

### <a name="windows-server-hyper-converged"></a>Windows Server, hiperconvergente

As tecnologias de virtualização do Windows Server incluem atualizações para o Hyper-V, Hyper-V Virtual Switch, Malha protegida e Máquinas virtuais protegidas (VMs), que melhoram a segurança, a escalabilidade e a confiabilidade. As atualizações de cluster de failover, rede e armazenamento facilitam ainda a implantação e o gerenciamento dessas tecnologias quando usadas com o Hyper-V.

![](media/sddc/spacer1.png)![](media/sddc/hyper-converged.png)

![](media/sddc/learn.png) **[Saiba mais sobre o Windows Server, hiperconvergente](https://docs.microsoft.com/windows-server/get-started/what-s-new-in-windows-server-2016#computevirtualizationvirtualizationmd)**

### <a name="hyper-v-hypervisor"></a>Hipervisor Hyper-V

O Hyper-V é uma tecnologia de virtualização com base em hipervisor para o Windows. O hipervisor é essencial para a virtualização. Ele é a plataforma de virtualização específica por processador que permite que vários sistemas operacionais isolados compartilhem uma única plataforma de hardware.

![](media/sddc/spacer1.png)![](media/sddc/hypervisor.png)

![](media/sddc/learn.png) **[Saiba mais sobre o hipervisor Hyper-V](https://www.microsoft.com/en-us/cloud-platform/server-virtualization)**

### <a name="guest-clustering-with-shared-vhdx"></a>Clustering de convidado com VHDX compartilhado

![](media/sddc/virtualize-line.png)

Flexível e seguro, e não vinculado à topologia de armazenamento subjacente, o VHDX compartilhado elimina a necessidade de apresentar o armazenamento físico subjacente para um sistema operacional convidado. O novo VHDX compartilhado oferece suporte ao redimensionamento online.

![](media/sddc/spacer1.png)![](media/sddc/cluster.png)

- O VHDX compartilhado pode residir em um Volume Compartilhado do Cluster (CSV) no armazenamento de bloco ou no armazenamento SMB com base em arquivos.
- Protegido: VHDX compartilhado dá suporte a backup em nível de host e réplica do Hyper-V.

![](media/sddc/learn.png) **[Saiba mais sobre Clustering de convidado com VHDX compartilhado](https://technet.microsoft.com/library/dn281956(v=ws.11).aspx)**

### <a name="hyper-v-replica"></a>Réplica do Hyper-V

![](media/sddc/virtualize-line.png)

Replicação integrada de VM com base em software pela rede com certificados. Não está vinculada ao hardware de servidor, rede ou armazenamento em qualquer um dos sites.

![](media/sddc/spacer1.png)![](media/sddc/replica.png)

Não requer outras tecnologias de replicação de máquina virtual, reduzindo os custos.
- Processa a migração dinâmica automaticamente.
- Gerenciamento e configuração simples: por meio do Gerenciador do Hyper-V, PowerShell ou com o Azure Site Recovery.

![](media/sddc/learn.png) **[Saiba mais sobre a réplica do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)**

![](media/sddc/networking.png)

### <a name="network-controller"></a>Controlador de rede

![](media/sddc/networking-line.png)

Um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter.

![](media/sddc/spacer1.png)![](media/sddc/netcontroller.png)

Os administradores usam uma Ferramenta de gerenciamento que interage diretamente com o Controlador de Rede. O controlador de rede fornece informações sobre a infraestrutura de rede, incluindo a infraestrutura física e virtual para a Ferramenta de gerenciamento.

![](media/sddc/learn.png) **[Saiba mais sobre o controlador de rede](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-controller/network-controller)**

### <a name="datacenter-firewall"></a>Firewall do datacenter

![](media/sddc/networking-line.png)

Quando implantado e oferecido como um serviço, os administradores locatários podem instalar e configurar políticas de firewall para ajudar a proteger redes virtuais de tráfego indesejado da Internet e redes de intranet.

![](media/sddc/spacer1.png)![](media/sddc/firewall.png)

O administrador do provedor de serviço ou o administrador de locatário pode gerenciar as políticas de Firewall do Datacenter pelo controlador de rede.

![](media/sddc/learn.png) **[Saiba mais sobre o Firewall do Datacenter](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/datacenter-firewall-overview)**

### <a name="switch-embedded-teaming"></a>Agrupamento incorporado de comutador

![](media/sddc/networking-line.png)

SET é uma solução alternativa de agrupamento NIC que você pode usar em ambientes que incluem o Hyper-V e a pilha de [Rede definida pelo software (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking).

![](media/sddc/spacer1.png)![](media/sddc/teaming.png)

![](media/sddc/learn.png) **[Saiba mais sobre o agrupamento incorporado do comutador](https://docs.microsoft.com/windows-server/networking/sdn/technologies/set-for-sdn)**

### <a name="software-load-balancing"></a>Balanceamento de carga do software

![](media/sddc/networking-line.png)

O SLB permite que vários servidores hospedem a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade. Dimensione os recursos de balanceamento de carga usando VMs de SLB nos mesmos servidores do Hyper-V que você usa para outras cargas de trabalho de VM. O SLB oferece suporte à criação rápida e à exclusão de pontos de extremidade de balanceamento de carga para operações de Provedor de serviços de nuvem. O SLB oferece suporte a dezenas de gigabytes por cluster, fornece um modelo simples de provisionamento e é fácil de dimensionar.

![](media/sddc/spacer1.png)![](media/sddc/balancer.png)

![](media/sddc/learn.png) **[Saiba mais sobre o balanceamento de carga de Software](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn)**


![](media/sddc/storage.png)

### <a name="storage-spaces-direct"></a>Espaços de Armazenamento Diretos

![](media/sddc/storage-line.png)

Usando os servidores padrão do setor com unidades conectadas localmente, os Espaços de armazenamento diretos fornecem armazenamento definido pelo software altamente disponível e escalonável com custo menor do que o de matrizes de SAN ou NAS tradicionais. Sua arquitetura simplifica radicalmente a aquisição e a implantação.

![Cada nó tem anexado localmente unidades em pool no nível do cluster por espaços de armazenamento diretos, em seguida, acessados pelas VMs por meio de CSVs](media/sddc/spacer1.png)![](media/sddc/ssd.png)

Os Espaços de armazenamento diretos introduzem o novo Barramento de armazenamento de software e aproveitam muitos dos recursos que você conhece hoje no Windows Server, como Clustering de Failover, Volumes Compartilhados do Cluster (CSVs)), protocolo SMB 3 e os Espaços de armazenamento.

![](media/sddc/learn.png) **[Saiba mais sobre espaços de armazenamento diretos](storage/storage-spaces/storage-spaces-direct-overview.md)**
### <a name="storage-quality-of-service"></a>Qualidade de Serviço do Armazenamento ###

![](media/sddc/storage-line.png)

Monitore e gerencie centralmente o desempenho de armazenamento de máquinas virtuais usando o Hyper-V e as funções de Servidor de Arquivos de Escalabilidade Horizontal, melhorando a justiça de recursos de armazenamento entre diversas máquinas virtuais.

![](media/sddc/spacer1.png)![](media/sddc/qos.png)

A Qualidade de serviço do armazenamento é integrada na solução de armazenamento definida por software da Microsoft fornecida pelo Servidor de Arquivos de Escalabilidade Horizontal e pelo Hyper-V usando o protocolo SMB3. Um novo Gerenciador de política oferece monitoramento de desempenho de armazenamento central.

![](media/sddc/learn.png) **[Saiba mais sobre a QoS de armazenamento](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview)**

### <a name="storage-replica"></a>Réplica de Armazenamento


![](media/sddc/storage-line.png)

A preparação e a recuperação de desastres possibilitam nenhuma perda de dados, com a capacidade de proteger os dados de modo síncrono em diferentes racks, andares, edifícios, campus, cidades e países com uso mais eficiente de vários datacenters.

![](media/sddc/spacer1.png)
![](media/sddc/storage-replica.png)

Replicação síncrona

1. O aplicativo grava dados
2. Dados de log são gravados e os dados são replicados para o local remoto
3. Dados de log são gravados no local remoto
4. Confirmação do local remoto
5. Gravação de aplicativo confirmada

t & t1: Dados liberados para o volume, sempre gravar os logs por meio de

![](media/sddc/learn.png) **[Saiba mais sobre a réplica de armazenamento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-overview)**

![](media/sddc/security.png)

### <a name="guarded-fabric"></a>Malha protegida

![](media/sddc/security-line.png)

Como um provedor de serviços em nuvem ou um administrador de nuvem privada corporativa, você pode usar uma malha protegida para fornecer um ambiente mais seguro para as VMs. Uma malha protegida consiste em um Serviço de Guardião de Host (HGS), em geral, um cluster de três nós, além de um ou mais hosts protegidos e um conjunto de máquinas virtuais protegidas (VMs).

![](media/sddc/spacer1.png)![](media/sddc/guarded-fabric.png)

![](media/sddc/learn.png) **[Saiba mais sobre a malha protegida](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### <a name="shielded-vms"></a>VMs blindadas

![](media/sddc/security-line.png)

Os dados e o estado de uma VM protegida são protegidos contra inspeção, roubo e adulteração por administradores de malware e datacenter.

![](media/sddc/spacer1.png)![](media/sddc/shielded.png)

- As VMs protegidas serão executados somente em malhas designadas como proprietárias da VM.
- As VMs protegidas são criptografadas pelo BitLocker ou outros meios para que somente os proprietários designados possam executá-las.
- As VMs em execução podem ser convertidas em protegidas.

![](media/sddc/learn.png) **[Saiba mais sobre as VMs blindadas](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### <a name="host-guardian-service"></a>Serviço Guardião de Host

![](media/sddc/security-line.png)

O Serviço Guardião de Host detém as chaves para malhas legítimas, bem como máquinas virtuais criptografadas.

![](media/sddc/spacer1.png)![](media/sddc/guardian.png)

![](media/sddc/learn.png) **[Saiba mais sobre o serviço guardião de Host](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-hgs)**

### <a name="device-health-attestation"></a>Atestado de integridade de dispositivo

![](media/sddc/security-line.png)

O atestado permite que as empresas elevem o nível de segurança de suas organizações para hardwares monitorados e segurança atestada, com pouco ou nenhum impacto nos custos da operação.


![](media/sddc/spacer1.png)![](media/sddc/attestation.png)


O modo confiável de hardware mostrado acima fornece o nível mais alto de garantia, com confiança proveniente de hardware do TPM v 2.0 e conformidade com a política de integridade de código para lançamento de chave.


![](media/sddc/learn.png) **[Saiba mais sobre o atestado de integridade do dispositivo](https://docs.microsoft.com/windows-server/security/device-health-attestation)**

![](media/sddc/management.png)

### <a name="powershell-desired-state-configuration"></a>PowerShell Desired State Configuration

![](media/sddc/management-line.png)

A Configuração de Estado Desejado do Windows PowerShell fornece uma plataforma de gerenciamento de configuração integrada no Windows que se baseia em padrões abertos. A DSC é flexível o suficiente para funcionar de maneira confiável e consistente em cada estágio do ciclo de vida de implantação (desenvolvimento, teste, pré-produção, produção), bem como durante a expansão.

![](media/sddc/spacer1.png)![](media/sddc/dsc.png)

A DSC oferece suporte às "implantações contínuas" para que você possa implantar configurações repetidamente sem perder nada.

-  As configurações de DSC se aplicam somente às configurações que foram alteradas em relação ao original para implantações mais rápidas.
-  O DSC pode ser usado no local, em um ambiente público ou em um ambiente privado de Nuvem.
-  Você pode integrar a DSC com qualquer solução da Microsoft ou não, desde que você possa executar um script do PowerShell no sistema de destino.

![](media/sddc/learn.png) **[Saiba mais sobre DSC do PowerShell](https://docs.microsoft.com/powershell/dsc/overview)**


### <a name="system-center-vmm"></a>System Center VMM

![](media/sddc/management-line.png)

O Virtual Machine Manager é parte do pacote do System Center, usado para configurar, gerenciar e transformar os datacenters tradicionais a fim de proporcionar uma experiência de gerenciamento unificada em todos os locais e provedor de serviços de nuvem do Azure.

![](media/sddc/spacer1.png)![](media/sddc/vmm.png)

- Datacenter: Configurar e gerenciar os componentes do datacenter como uma única malha no VMM. 
- Hosts de virtualização: O VMM pode adicionar, provisionar e gerenciar e clusters do Hyper-V e hosts de virtualização do VMware.
- Rede: O VMM fornece virtualização de rede, incluindo suporte para criar e gerenciar redes virtuais e gateways de rede. 
- Armazenamento: O VMM pode descobrir, classificar, provisionar, alocar e atribuir o armazenamento local e remoto.

![](media/sddc/learn.png) **[Saiba mais sobre o System Center VMM](https://docs.microsoft.com/system-center/vmm/)**

### <a name="windows-admin-center"></a>Windows Admin Center

![](media/sddc/management-line.png)

O Windows Admin Center é um conjunto de ferramentas de gerenciamento implantado localmente, com base em navegador, que permite a administração no local de servidores do Windows sem dependência do Azure ou nuvem. O Windows Admin Center fornece aos administradores de TI controle total sobre todos os aspectos da infraestrutura de servidor e é particularmente útil para o gerenciamento de redes privadas que não estão conectadas à Internet.

![](media/sddc/spacer1.png)![](media/sddc/architecture.png)

A publicação do servidor Web no DNS e a configuração do firewall corporativo permitem que você acesse o Windows Admin Center pela rede pública, permitindo que você se conecte e gerencie os servidores de qualquer lugar com o Microsoft Edge ou o Google Chrome.

![](media/sddc/learn.png) **[Saiba mais sobre o Windows Admin Center](manage/windows-admin-center/overview.md)**
