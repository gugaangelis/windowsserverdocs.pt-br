---
title: Requisitos de hardware e opções de armazenamento de clustering de failover
description: Requisitos de hardware e opções de armazenamento para criar um cluster de failover.
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 65d2d21138efbae3c5ada56bfa8628f06c4dcbe7
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990802"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Requisitos de hardware e opções de armazenamento de clustering de failover

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você precisa do seguinte hardware para criar um cluster de failover. Para receber suporte da Microsoft, todo o hardware deve ser certificado para a versão do Windows Server que você está executando, e a solução de cluster de failover completa deve passar por todos os testes no Assistente para Validar Configuração. Para obter mais informações sobre como validar um cluster de failover, consulte [Validar hardware para um cluster de failover](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Servidores**: É recomendável usar um conjunto de computadores correspondentes que contenham os mesmos ou componentes similares.
- **Adaptadores e cabo de rede (para comunicação de rede)**: se você usar o iSCSI, cada adaptador de rede deverá ser dedicado para comunicação de rede ou ao iSCSI, não ambos.

    Na infraestrutura de rede que conecta os nós do cluster, evite ter pontos de falha únicos. Por exemplo, é possível conectar os nós do cluster por várias redes distintas. Como alternativa, você pode conectar seus nós de cluster com uma rede que é construída com adaptadores de rede agrupados, comutadores redundantes, roteadores redundantes ou hardware semelhante que remove pontos únicos de falha.

    >[!NOTE]
    >Se você conectar nós do cluster com uma única rede, a rede passará no requisito de redundância do Assistente para Validar Configuração. No entanto, o relatório do assistente incluirá um aviso informando que a rede não deve ter pontos de falha únicos.

- **Controladores de dispositivo ou adaptadores adequados para o armazenamento**:

  - **Serial Attached SCSI ou Fibre Channel**: se você estiver usando Serial Attached SCSI ou Fibre Channel, em todos os servidores clusterizados, todos os elementos da pilha de armazenamento deverão ser idênticos. É necessário que o software de e/s de vários caminhos (MPIO) seja idêntico e que o software do módulo específico do dispositivo (DSM) seja idêntico. É recomendável que os controladores de dispositivo de armazenamento em massa — o HBA (adaptador de barramento de host), os drivers de HBA e o firmware do HBA — que são anexados ao armazenamento de cluster sejam idênticos. Se usar HBAs diferentes, você deverá verificar, com o fornecedor de armazenamento, se está cumprindo as configurações com suporte ou recomendadas.
  - **iSCSI**: Se você estiver usando iSCSI, cada servidor clusterizado deverá ter um ou mais adaptadores de rede ou HBAs dedicados para o armazenamento de cluster. A rede usada para o iSCSI não deve ser usada para comunicações de rede. Em todos os servidores clusterizados, os adaptadores de rede usados para conexão com o destino de armazenamento iSCSI devem ser idênticos. É recomendável também usar Gigabit Ethernet ou superior.
- **Armazenamento**: você deve usar [espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-overview.md) ou armazenamento compartilhado que seja compatível com o Windows Server 2012 R2 ou o Windows Server 2012. Você pode usar o armazenamento compartilhado que está anexado e também pode usar compartilhamentos de arquivos SMB 3,0 como armazenamento compartilhado para servidores que executam o Hyper-V que estão configurados em um cluster de failover. Para obter mais informações, consulte [Implantar Hyper-V no SMB](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    Na maioria dos casos, o armazenamento anexado deve conter múltiplos discos separados (números de unidade lógica, ou LUNs) configurados no nível do hardware. Para alguns clusters, um disco funciona como a testemunha de disco (descrito no final desta subseção). Outros discos contêm os arquivos necessários para as funções clusterizadas (antigamente chamado de serviços ou aplicativos clusterizados). Os requisitos de armazenamento incluem o seguinte:

  - Para usar o suporte de disco nativo incluído no Clustering de Failover, use discos básicos, não discos dinâmicos.
  - É recomendável formatar a partição com NTFS. Se você usa CSV (Volumes Compartilhados do Cluster), a partição de cada um deverá ser NTFS.

    >[!NOTE]
    >Se você tiver uma testemunha de disco para a sua configuração de quorum, poderá formatar o disco com NTFS ou ReFS (Sistema de Arquivos Resiliente).

  - Para o estilo de partição do disco, é possível usar MBR (registro mestre de inicialização) ou GPT (tabela de partição GUID).

    Uma testemunha de disco é um disco no armazenamento de cluster designado para manter uma cópia do banco de dados de configuração de cluster. Um cluster de failover tem uma testemunha de disco somente se isto for especificado como parte da configuração de quórum. Para obter mais informações, consulte [Understanding quorum in espaços de armazenamento diretos](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Requisitos de hardware para Hyper-V

Se você estiver criando um cluster de failover que inclui máquinas virtuais clusterizadas, os servidores de cluster deverão dar suporte aos requisitos de hardware para a função Hyper-V. O Hyper-V requer um processador de 64 bits que inclua:

- Virtualização assistida por hardware. Isso está disponível em processadores que incluem uma opção de virtualização, especialmente aqueles com a tecnologia Intel VT (Intel Virtualization) ou AMD-V (AMD Virtualization).
- A DEP (Prevenção de Execução de Dados) imposta por hardware deve estar disponível e habilitada. Especificamente, você deve habilitar o bit Intel XD (bit execute disable) ou o bit AMD NX (bit no execute).

Para obter mais informações sobre a função Hyper-V, consulte [Visão geral do Hyper-V](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implantando redes de área de armazenamento com clusters de failover

Ao implantar uma rede de área de armazenamento (SAN) com um cluster de failover, siga estas diretrizes:

- **Confirme a compatibilidade do armazenamento**: Confirme com os fabricantes e fornecedores se o armazenamento, incluindo drivers, firmware e software utilizados para o armazenamento, são compatíveis com clusters de failover na versão do Windows Server que você está executando.
- **Isolar dispositivos de armazenamento, um cluster por dispositivo**: os servidores de diferentes clusters podem não conseguir acessar os mesmos dispositivos de armazenamento. Na maioria dos casos, um LUN usado para um conjunto de servidores clusterizados deve ser isolado de todos os outros servidores através de uma máscara de LUN ou divisão em zonas.
- **Considere usar software de E/S de vários caminhos ou adaptadores de rede emparelhados**: em uma malha de armazenamento altamente disponível, você pode implantar clusters de failover com várias controladoras usando software de E/S de vários caminhos ou adaptadores de rede emparelhados (também chamado de balanceamento de carga e failover, ou LBFO). Isso fornece o mais alto nível de redundância e disponibilidade. Para o Windows Server 2012 R2 ou o Windows Server 2012, sua solução de vários caminhos deve ser baseada no Microsoft Multipath I/O (MPIO). O fornecedor de hardware normalmente fornece um DSM (módulo específico ao dispositivo) do MPIO para o hardware, embora o Windows Server inclua um ou mais DSMs como parte do sistema operacional.

    Para obter mais informações sobre o LBFO, consulte [visão geral do agrupamento NIC](../networking/technologies/nic-teaming/nic-teaming.md) na biblioteca técnica do Windows Server.

    >[!IMPORTANT]
    >As controladoras e o software MPIO podem fazer muita diferenciação de versões. Se você estiver implementando uma solução multipath para o cluster, trabalhe em sintonia com o fornecedor de hardware para escolher os adaptadores, firmware e software corretos para a versão do Windows Server que você está executando.

- **Considere o uso de espaços de armazenamento**: se você planeja implantar o armazenamento em cluster SAS (Serial Attached SCSI) configurado usando espaços de armazenamento, consulte [implantar espaços de armazenamento em cluster](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) para os requisitos.

## <a name="more-information"></a>Mais informações

- [Clustering de failover](./failover-clustering-overview.md)
- [Espaços de armazenamento](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Usando clustering convidado para alta disponibilidade](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)