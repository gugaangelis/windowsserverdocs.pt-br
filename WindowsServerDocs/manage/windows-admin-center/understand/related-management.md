---
title: Windows Admin Center relacionadas a soluções de gerenciamento
description: Como o Windows Admin Center se compara com e complementa outros monitoramento e gerenciamento de soluções/produtos da Microsoft (projeto Paulo)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 385a066cb828f58d698c2ca47e0553e996a77733
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847317"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center e soluções de gerenciamento relacionados da Microsoft

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

[Windows Admin Center](windows-admin-center.md) é a evolução do servidor do tradicional na caixa de ferramentas de gerenciamento para situações em que você pode ter usado da área de trabalho remota (RDP) para se conectar a um servidor de configuração ou solução de problemas. Ele não deve substituir outras soluções de gerenciamento existentes do Microsoft; em vez disso, ela complementa essas soluções, conforme descrito abaixo.

## <a name="remote-server-administration-tools-rsat"></a>Ferramentas de Administração de Servidor Remoto (RSAT)

[Ferramentas de administração de servidor remoto (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) é uma coleção de ferramentas de GUI e o PowerShell para gerenciar as funções opcionais e recursos no Windows Server. RSAT possui muitos recursos que não tenha o Windows Admin Center. Poderemos adicionar algumas das ferramentas mais usadas em RSAT para Windows Admin Center no futuro. Qualquer nova função de servidor do Windows ou um recurso que requer uma interface gráfica para gerenciamento será no Windows Admin Center.

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) é um serviço de gerenciamento de mobilidade empresarial baseado em nuvem que permite que você gerencie dispositivos iOS, Android, Windows e macOS, com base em um conjunto de políticas. Intune concentra-se sobre como habilitar a proteção de informações da empresa controlando como sua força de trabalho acessa e compartilha informações. Em contraste, Windows Admin Center não é controlado por política, mas permite que o gerenciamento do ad-hoc de sistemas Windows 10 e Windows Server, usando o PowerShell e o WMI remoto sobre o WinRM.

## <a name="azure-stack"></a>Azure Stack

[O Azure Stack](https://azure.microsoft.com/overview/azure-stack/) é uma plataforma de nuvem híbrida que possibilita entregar serviços do Azure do seu data center. O Azure Stack é gerenciado usando o PowerShell ou o portal do administrador, que é semelhante ao portal do Azure tradicional usado para acessar e gerenciar serviços do Azure tradicionais. Windows Admin Center não é destinado para gerenciar a infraestrutura do Azure Stack, mas você pode usá-lo para [gerenciar máquinas virtuais de IaaS do Azure](../configure/manage-azure-vms.md) (executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012) ou solucionar problemas servidores físicos individuais implantados em seu ambiente do Azure Stack.

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) é uma solução de gerenciamento do Centro de dados local para implantação, configuração, gerenciamento, monitoramento de seu data center inteiro. System Center lhe permite visualizar o status de todos os sistemas em seu ambiente, enquanto o Windows Admin Center lhe permite fazer uma busca detalhada em um servidor específico para gerenciar ou solução de problemas com as ferramentas mais granulares.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Ferramentas e plataforma de "na caixa" reimaginada** | **Gerenciamento de data center e monitoramento** |
| Incluído com a licença do Windows Server – **sem nenhum custo adicional**, assim como o MMC e outras ferramentas tradicionais de caixa de entrada | **Abrangente** pacote de soluções de valor adicional em seu ambiente e plataformas |
| **Lightweight**, baseada em navegador e o gerenciamento remoto de instâncias do Windows Server, **em qualquer lugar**; alternativo para o RDP | Gerenciar e monitorar **heterogêneos** sistemas **em escala**, incluindo Linux, VMware e Hyper-V |
|**Profundo** servidor único & único cluster drill down para solução de problemas, configuração e manutenção|Infraestrutura de provisionamento; automação e autoatendimento;  infraestrutura e carga de trabalho de monitoramento **amplitude**|
|Com otimização de gerenciamento do **individuais** nó 2 a 4 **HCI** clusters, a integração do Hyper-V, espaços de armazenamento diretos e SDN|Implantar e gerenciar o Hyper-V, clusters do Windows Server em **escala do datacenter** partir **bare-metal** com o SCVMM|
|**Monitoramento em HCI** apenas; o serviço de integridade do cluster armazena o histórico. Plataforma extensível para terceiros 1º e o 3º **as extensões de ferramentas de administração**|**Extensível** & **monitoramento escalonável** plataforma no SCOM, com alertas, notificações, carga de trabalho de terceiros monitoramento; SQL para o histórico|
|Ponte mais fácil para **híbrida**; integrar e usar uma variedade de serviços do Azure para proteção de dados, replicação, atualizações e muito mais|**Interno** proteção de dados, replicação, as atualizações (VMM/DPM/SCCM). Integração híbrida com o Log Analytics e o mapa do serviço|
|**Acende recursos da plataforma** do Windows Server: Storage Migration Service, Storage Replica, System Insights, etc.|**Outras plataformas**: Automação no Orchestrator/SMA. Integrações com SCSM & outras ferramentas de gerenciamento de serviço|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>Cada uma delas fornece o valor de destino de forma independente; **melhores juntos** com recursos complementares.
