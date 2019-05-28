---
title: Requisitos de hardware de clustering de failover e opções de armazenamento
description: Requisitos de hardware e opções de armazenamento para criar um cluster de failover.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: e6eb6f2acd420ae657a5c1b698e9733751378552
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476068"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Requisitos de hardware de clustering de failover e opções de armazenamento

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você precisa do seguinte hardware para criar um cluster de failover. Para receber suporte da Microsoft, todo o hardware deve ser certificado para a versão do Windows Server que você está executando, e a solução de cluster de failover completa deve passar por todos os testes no Assistente para Validar Configuração. Para obter mais informações sobre como validar um cluster de failover, consulte [Validar hardware para um cluster de failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Servidores**: É recomendável usar um conjunto de computadores correspondentes que contenham os mesmos ou componentes similares.
- **Adaptadores e cabo de rede (para comunicação de rede)** : Se você usa iSCSI, cada adaptador de rede deve ser dedicado à comunicação de rede ou iSCSI, não para ambos.

    Na infraestrutura de rede que conecta os nós do cluster, evite ter pontos de falha únicos. Por exemplo, é possível conectar os nós do cluster por várias redes distintas. Como alternativa, você pode conectar os nós de cluster com uma rede construída com adaptadores de rede emparelhados, comutadores redundantes, roteadores redundantes ou hardware semelhante que remova pontos únicos de falha.

    >[!NOTE]
    >Se você conectar nós do cluster com uma única rede, a rede passará no requisito de redundância do Assistente para Validar Configuração. No entanto, o relatório do assistente incluirá um aviso informando que a rede não deve ter pontos de falha únicos.

- **Controladores de dispositivo ou adaptadores adequados para o armazenamento**:

  - **SAS ou Fibre Channel**: Se você estiver usando SAS ou Fibre Channel em todos os servidores clusterizados, todos os elementos da pilha de armazenamento deverão ser idênticos. É necessário que o software multipath de e/s (MPIO) seja idêntico e que o software de módulo específico de dispositivo (DSM) sejam idênticos. É recomendável que os controladores de dispositivo de armazenamento em massa — controladora (HBA), drivers HBA e firmware HBA de barramento de host — que estão anexados ao armazenamento de cluster sejam idênticos. Se usar HBAs diferentes, você deverá verificar, com o fornecedor de armazenamento, se está cumprindo as configurações com suporte ou recomendadas.
  - **iSCSI**: Se você estiver usando iSCSI, cada servidor com cluster deverá ter um ou mais adaptadores de rede ou HBAs dedicados para o armazenamento de cluster. A rede usada para o iSCSI não deve ser usada para comunicações de rede. Em todos os servidores clusterizados, os adaptadores de rede usados para conexão com o destino de armazenamento iSCSI devem ser idênticos. É recomendável também usar Gigabit Ethernet ou superior.
- **Armazenamento**: Você deve usar [espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-overview.md) ou compartilhado de armazenamento que seja compatível com o Windows Server 2012 R2 ou Windows Server 2012. Você pode usar o armazenamento compartilhado que está anexado, e você também pode usar compartilhamentos de arquivos SMB 3.0 como armazenamento compartilhado para servidores que executam o Hyper-V que são configurados em um cluster de failover. Para obter mais informações, consulte [Implantar Hyper-V no SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    Na maioria dos casos, o armazenamento anexado deve conter múltiplos discos separados (números de unidade lógica, ou LUNs) configurados no nível do hardware. Para alguns clusters, um disco funciona como a testemunha de disco (descrito no final desta subseção). Outros discos contêm os arquivos necessários para as funções clusterizadas (antigamente chamado de serviços ou aplicativos clusterizados). Os requisitos de armazenamento incluem o seguinte:

  - Para usar o suporte de disco nativo incluído no Clustering de Failover, use discos básicos, não discos dinâmicos.
  - É recomendável formatar a partição com NTFS. Se você usa CSV (Volumes Compartilhados do Cluster), a partição de cada um deverá ser NTFS.

    >[!NOTE]
    >Se você tiver uma testemunha de disco para a sua configuração de quorum, poderá formatar o disco com NTFS ou ReFS (Sistema de Arquivos Resiliente).

  - Para o estilo de partição do disco, é possível usar MBR (registro mestre de inicialização) ou GPT (tabela de partição GUID).

    Uma testemunha de disco é um disco no armazenamento de cluster designado para manter uma cópia do banco de dados de configuração de cluster. Um cluster de failover tem uma testemunha de disco somente se isto for especificado como parte da configuração de quórum. Para obter mais informações, consulte [Noções básicas sobre o Quorum em espaços de armazenamento diretos](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Requisitos de hardware para Hyper-V

Se você estiver criando um cluster de failover que inclui máquinas virtuais clusterizadas, os servidores de cluster deverão dar suporte aos requisitos de hardware para a função Hyper-V. O Hyper-V requer um processador de 64 bits que inclua:

- Virtualização assistida por hardware. Isso está disponível em processadores que incluem uma opção de virtualização, especialmente aqueles com a tecnologia Intel VT (Intel Virtualization) ou AMD-V (AMD Virtualization).
- A DEP (Prevenção de Execução de Dados) imposta por hardware deve estar disponível e habilitada. Especificamente, você deve habilitar o bit Intel XD (bit execute disable) ou o bit AMD NX (bit no execute).

Para obter mais informações sobre a função Hyper-V, consulte [Visão geral do Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implantando redes de área de armazenamento com clusters de failover

Ao implantar uma rede de área de armazenamento (SAN) com um cluster de failover, siga estas diretrizes:

- **Confirme a compatibilidade do armazenamento**: Confirme com os fabricantes e fornecedores se o armazenamento, incluindo drivers, firmware e software utilizados para o armazenamento, são compatíveis com clusters de failover na versão do Windows Server que você está executando.
- **Isole dispositivos de armazenamento, um cluster por dispositivo**: Servidores de diferentes clusters não devem poder acessar os mesmos dispositivos de armazenamento. Na maioria dos casos, um LUN usado para um conjunto de servidores clusterizados deve ser isolado de todos os outros servidores através de uma máscara de LUN ou divisão em zonas.
- **Considere o uso de software MPIO ou adaptadores de rede atrelados**: Em uma malha de armazenamento altamente disponível, você pode implantar clusters de failover com vários adaptadores de barramento do host usando software MPIO ou agrupamento de adaptadores de rede (também chamado de balanceamento de carga e failover, ou LBFO). Isso fornece o mais alto nível de redundância e disponibilidade. Para o Windows Server 2012 R2 ou Windows Server 2012, sua solução multipath deve ser baseada no Microsoft Multipath i / (MPIO). O fornecedor de hardware normalmente fornece um DSM (módulo específico ao dispositivo) do MPIO para o hardware, embora o Windows Server inclua um ou mais DSMs como parte do sistema operacional.

    Para obter mais informações sobre LBFO, consulte [visão geral do agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) na biblioteca técnica do Windows Server.

    >[!IMPORTANT]
    >As controladoras e o software MPIO podem fazer muita diferenciação de versões. Se você estiver implementando uma solução multipath para o cluster, trabalhe em sintonia com o fornecedor de hardware para escolher os adaptadores, firmware e software corretos para a versão do Windows Server que você está executando.

- **Considere o uso de Espaços de Armazenamento**: Se você planeja implantar serial anexado SCSI (SAS) clusterizado armazenamento que é configurado usando espaços de armazenamento, consulte [Deploy Clustered Storage Spaces](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) para os requisitos.

## <a name="more-information"></a>Mais informações

- [Clustering de failover](failover-clustering.md)
- [Espaços de armazenamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Using Guest Clustering for High Availability](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)