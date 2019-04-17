---
title: Opções de armazenamento e requisitos de hardware de cluster de failover
description: Requisitos de hardware e as opções de armazenamento para a criação de um cluster de failover.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4706372b06d0554196b692c3ddcda145dee5bae5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081811"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Opções de armazenamento e requisitos de hardware de cluster de failover

Aplica-se a: Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2016

Você precisa de hardware a seguir para criar um cluster de failover. Para ser suportada pela Microsoft, todo o hardware deve ser certificado para a versão do Windows Server que você está executando e a solução de cluster de failover completa deve passar todos os testes em validar um Assistente de configuração. Para obter mais informações sobre como validar um cluster de failover, consulte [Validar o Hardware para um Cluster de Failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Servidores**: Recomendamos que você use um conjunto de computadores correspondentes que contenha os mesmos componentes ou semelhantes.
- **Cabo (para comunicação de rede) e adaptadores de rede**: se você usar iSCSI, cada adaptador de rede deve ser dedicado a comunicação de rede ou iSCSI, não ambos.

    Na infraestrutura de rede que conecta os nós de cluster, evite ter pontos únicos de falha. Por exemplo, você pode conectar os nós de cluster por vários redes distintas. Como alternativa, você pode conectar os nós de cluster com uma rede que é construída com adaptadores de rede emparelhados, comutadores redundantes, roteadores redundantes ou hardware similar que remova pontos únicos de falha.

    >[!NOTE]
    >Se você conectar nós de cluster com uma única rede, a rede passará o requisito de redundância em validar um Assistente de configuração. No entanto, o relatório do assistente incluirá um aviso de que a rede não deveria ter pontos únicos de falha.

- **Os controladores de dispositivo ou adaptadores apropriados para o armazenamento**:

  - **Serial Attached SCSI ou Fibre Channel**: se você estiver usando um Serial Attached SCSI ou Fibre Channel, em servidores clusterizados tudo, todos os elementos da pilha de armazenamento devem ser idênticos. É obrigatório se o software de e/s (MPIO) vários caminhos ser idêntica e que o software do módulo específico do dispositivo (DSM) seja idêntico. Recomenda-se que os controladores de dispositivo de armazenamento em massa — o host de barramento adaptador (HBA), drivers HBAs e firmware do HBA — que estão associadas ao armazenamento em cluster ser idêntico. Se você usar HBAs tipos diferentes, você deve verificar com o fornecedor de armazenamento que você esteja seguindo suas configurações com suportadas ou recomendadas.
  - **iSCSI**: se você estiver usando iSCSI, cada servidor em cluster deve ter um ou mais adaptadores de rede ou HBAs dedicados ao armazenamento de cluster. A rede que você usa para iSCSI não deve ser usada para comunicação de rede. Nas clustered todos os servidores, os adaptadores de rede que você pode usar para se conectar ao destino do armazenamento iSCSI devem ficar idênticos e é recomendável que você use Ethernet Gigabit ou superior.
- **Armazenamento**: você deve usar [Espaços de armazenamento direto](../storage/storage-spaces/storage-spaces-direct-overview.md) ou que seja compatível com o Windows Server 2012 R2 ou Windows Server 2012 armazenamento compartilhado. Você pode usar o armazenamento compartilhado anexado, e você também pode usar os compartilhamentos de arquivos SMB 3.0 como armazenamento compartilhado para servidores que estão executando o Hyper-V e que estão configurados em um cluster de failover. Para obter mais informações, consulte [Implantar o Hyper-V em SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    Na maioria dos casos, o armazenamento anexado deve conter vários, separar discos (números de unidades lógicas ou LUNs) que são configurados no nível de hardware. Para alguns clusters, um disco funciona como testemunha de disco (descrita no final desta subseção). Outros discos contêm os arquivos necessários para as funções clusterizadas (anteriormente denominadas serviços ou aplicativos clusterizados). Requisitos de armazenamento incluem o seguinte:

  - Para usar o suporte a disco nativo incluído no cluster de Failover, use discos básicos, não discos dinâmicos.
  - Recomendamos que você formatar as partições com NTFS. Se você usar o Cluster compartilhados Volumes (CSV), a partição para cada um deles deve ser NTFS.

    >[!NOTE]
    >Se você tiver uma testemunha de disco para sua configuração de quorum, você pode formatar o disco com um sistema de arquivos resiliente (ReFS) ou NTFS.

  - Para o estilo de partição do disco, você pode usar o registro mestre de inicialização (MBR) ou tabela de partição GUID (GPT).

    Uma testemunha de disco é um disco do armazenamento de cluster que tiver designado para manter uma cópia do banco de dados de configuração do cluster. Um cluster de failover tem uma testemunha de disco somente se for especificado como parte da configuração de quorum. Para obter mais informações, consulte [Noções básicas sobre o Quorum em espaços de armazenamento direto](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Requisitos de hardware para Hyper-V

Se você estiver criando um cluster de failover que inclui as máquinas virtuais em cluster, os servidores de cluster devem suportar os requisitos de hardware para a função Hyper-V. Hyper-V exige um processador de 64 bits que inclui o seguinte:

- Virtualização assistida por hardware. Essa opção estará disponível em processadores que incluem uma opção de virtualização — especificamente, processadores com Intel Virtualization Technology (Intel VT) ou uma tecnologia AMD Virtualization (AMD-V).
- Prevenção de execução de dados (DEP) reforçada pelo hardware deve estar disponível e habilitada. Especificamente, você deve habilitar o bit Intel XD (bit execute disable) ou o bit AMD NX (nenhum bit execute).

Para obter mais informações sobre a função do Hyper-V, consulte [Visão geral do Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implantando redes de área de armazenamento com clusters de failover

Ao implantar uma rede de área de armazenamento (SAN) com um cluster de failover, siga estas diretrizes:

- **Confirmar compatibilidade do armazenamento**: confirmar com os fabricantes e fornecedores que o armazenamento, incluindo os drivers, firmware e software usado para o armazenamento, são compatíveis com os clusters de failover na versão do Windows Server que você está executando.
- **Isolar dispositivos de armazenamento, um cluster por dispositivo**: servidores de clusters diferentes não devem ser capazes de acessar os mesmos dispositivos de armazenamento. Na maioria dos casos, um LUN usado para um conjunto de servidores de cluster deve ser isolado de todos os outros servidores por meio de LUN mascaramento ou divisão em zonas.
- **Considere utilizar o software de e/s de vários caminhos ou parceria adaptadores de rede**: em uma estrutura de armazenamento altamente disponível, você pode implantar clusters de failover com vários adaptadores de barramento de host usando o software de e/s de vários caminhos ou adaptador agrupamento (também chamado de carga de rede balanceamento e failover ou LBFO). Isso oferece o nível mais alto de disponibilidade e redundância. Para o Windows Server 2012 R2 ou Windows Server 2012, sua solução de vários caminhos deve se basear em e/s de vários caminhos da Microsoft (MPIO). Seu fornecedor de hardware geralmente fornecerá um módulo de específicas de dispositivo MPIO (DSM) para o seu hardware, embora o Windows Server inclui um ou mais DSMs como parte do sistema operacional.

    Para obter mais informações sobre LBFO, consulte [Visão geral de agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) na biblioteca técnica do Windows Server.

    >[!IMPORTANT]
    >Adaptadores de barramento de host e o software de e/s de vários caminhos podem ser muito versão confidencial. Se você estiver implementando uma solução de vários caminhos para o cluster, trabalham em conjunto com seu fornecedor de hardware para escolher os adaptadores corretos, firmware e software para a versão do Windows Server que você está executando.

- **Convém usar espaços de armazenamento**: se você pretende implantar serial armazenamento anexado SAS (SCSI) clusterizado que é configurado usando espaços de armazenamento, consulte [Implantar espaços de armazenamento em cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) para saber os requisitos.

## <a name="more-information"></a>Mais informações

- [Clustering de failover](failover-clustering.md)
- [Espaços de Armazenamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Usar o Clustering de Convidado para Alta Disponibilidade](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)