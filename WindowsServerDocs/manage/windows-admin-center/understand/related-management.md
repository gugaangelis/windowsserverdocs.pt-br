---
title: Soluções de gerenciamento relacionadas ao Windows Admin Center
description: Como o Windows Admin Center se compara e complementa outras soluções/produtos de monitoramento e gerenciamento da Microsoft (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d681e5007cd3ae3c14de774df0bc85abc23b51d7
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371678"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center e soluções de gerenciamento relacionadas da Microsoft

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

O [Windows Admin Center](windows-admin-center.md) é a evolução das tradicionais ferramentas de gerenciamento de servidor prontas para uso para situações nas quais você pode ter usado a Área de Trabalho Remota (RDP) para se conectar a um servidor para solução de problemas ou configuração. Ele não deve substituir outras soluções de gerenciamento existentes da Microsoft. Em vez disso, ele complementa essas soluções, conforme descrito abaixo.

## <a name="remote-server-administration-tools-rsat"></a>Ferramentas de Administração de Servidor Remoto (RSAT)

As [Ferramentas de Administração de Servidor Remoto (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) são uma coleção de ferramentas de do PowerShell e de interface gráfica do usuário para gerenciar as funções e os recursos opcionais no Windows Server. As RSAT têm muitos recursos que o Windows Admin Center não tem. Poderemos adicionar algumas das ferramentas mais usadas nas RSAT ao Windows Admin Center no futuro. Qualquer nova função de Servidor do Windows ou recurso que precise de uma interface gráfica para gerenciamento estará no Windows Admin Center.

## <a name="intune"></a>Intune

O [Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) é um serviço de gerenciamento de mobilidade empresarial baseado em nuvem que permite que você gerencie dispositivos iOS, Android, Windows e macOS, com base em um conjunto de políticas. O Intune se concentra em permitir que você proteja as informações da empresa controlando como sua força de trabalho acessa e compartilha informações. Em contraste, o Windows Admin Center não é controlado por política, mas permite o gerenciamento ad-hoc de sistemas Windows 10 e Windows Server usando o PowerShell e o WMI remoto por WinRM.

## <a name="azure-stack"></a>Azure Stack

O [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) é uma plataforma de nuvem híbrida que possibilita distribuir serviços do Azure por seu data center. O Azure Stack é gerenciado usando o PowerShell ou o portal do administrador, que é semelhante ao portal do Azure tradicional usado para acessar e gerenciar serviços do tradicionais do Azure. O Windows Admin Center não se destina a gerenciar a infraestrutura do Azure Stack, mas você pode usá-lo para [gerenciar máquinas virtuais de IaaS do Azure](../azure/manage-azure-vms.md) (executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012) ou solucionar problemas de servidores físicos individuais implantados em seu ambiente do Azure Stack.

## <a name="system-center"></a>System Center

O [System Center](https://www.microsoft.com/cloud-platform/system-center) é uma solução de gerenciamento de data center no local para implantação, configuração, gerenciamento, monitoramento de seu data center inteiro. O System Center permite visualizar o status de todos os sistemas em seu ambiente, enquanto o Windows Admin Center permite fazer uma busca detalhada em um servidor específico para gerenciar ou solucionar problemas com as ferramentas mais granulares.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Ferramentas e plataforma “prontas” reimaginadas** | **Monitoramento e gerenciamento do data center** |
| Incluído com a licença do Windows Server – **sem nenhum custo adicional**, assim como o MMC e outras ferramentas prontas tradicionais | **Abrangente** pacote de soluções de valor adicional em seu ambiente e plataformas |
| **Leve**, gerenciamento remoto baseado em navegador de instâncias do Windows Server, **em qualquer lugar**; alternativa ao RDP | Gerenciar e monitorar sistemas **heterogêneos** **em escala**, incluindo Linux, VMware e Hyper-V |
|**Detalhado** detalhamento de servidor e cluster únicos para solução de problemas, configuração e manutenção|Infraestrutura de provisionamento; automação e autoatendimento; **amplitude** de infraestrutura e carga de trabalho de monitoramento|
|Gerenciamento otimizado **individual** de clusters **HCI** de 2 a 4 nós, integrando Hyper-V, Espaços de Armazenamento Diretos e SDN|Implantar e gerenciar o Hyper-V, clusters do Windows Server em **escala do data center** de **bare-metal** com o SCVMM|
|Somente **Monitoramento em HCI**; serviço de integridade do cluster armazena o histórico. Plataforma extensível para **extensões de ferramentas de administração** próprias e de terceiros|Plataforma de monitoramento escalonável **Extensível** &  **no SCOM**, com alertas, notificações, monitoramento de carga de trabalho de terceiros; SQL para o histórico|
|Ponte mais fácil para o **híbrido**; integrar e usar uma variedade de serviços do Azure para proteção de dados, replicação, atualizações e muito mais|Proteção, replicação, atualizações **internas** de dados (VMM/DPM/SCCM). Integração híbrida com Análise de Log e Mapa do Serviço|
|**Ilumina os recursos de plataforma** do Windows Server: serviço de migração de armazenamento, réplica de armazenamento, insights do sistema, etc.|**Plataformas adicionais**: automação no Orchestrator/sma. Integrações com o SCSM & outras ferramentas de gerenciamento de serviços|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>Cada uma delas fornece o valor de destino de forma independente; **melhores juntos** com recursos complementares.
