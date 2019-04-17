---
title: Soluções de gerenciamento relacionadas ao Windows Admin Center
description: Como o Windows Admin Center compara com e complementa outras monitoramento e gerenciamento soluções/produtos da Microsoft (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7bf0e32b1156fe361c79ac4ccd0e3536df767e2
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296708"
---
# Windows Admin Center e soluções de gerenciamento relacionados da Microsoft

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

[Windows Admin Center](windows-admin-center.md) é a evolução das ferramentas de gerenciamento de servidor de caixa de entrada tradicionais para situações em que você pode ter usado da área de trabalho remota (RDP) para se conectar a um servidor para solução de problemas ou configuração. Não é destinado a substituir outras soluções de gerenciamento Microsoft existentes; em vez disso, ele complementa essas soluções, conforme descrito a seguir.

## Ferramentas de Administração de Servidor Remoto (RSAT)

[Ferramentas de administração de servidor remoto (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) é uma coleção de ferramentas de GUI e do PowerShell para gerenciar opcionais funções e recursos no Windows Server. RSAT tem muitos recursos que não têm o Windows Admin Center. Poderemos adicionar algumas das ferramentas mais comumente usadas no RSAT no Windows Admin Center no futuro. Qualquer nova função de servidor Windows ou recurso que requer uma interface gráfica para gerenciamento será no Centro de administração do Windows.

## Intune

[O Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) é um serviço de gerenciamento de mobilidade empresarial baseado em nuvem que permite que você gerencie dispositivos iOS, Android, Windows e macOS, com base em um conjunto de políticas. Intune se concentra em como habilitar a proteção de informações da empresa, controlando como sua força de trabalho acessa e compartilha informações. Por outro lado, o Windows Admin Center não é orientado por políticas, mas permite que o gerenciamento de ad-hoc de sistemas Windows 10 e Windows Server, usando o PowerShell e WMI remoto WinRM.

## Azure Stack

[Pilha do Azure](https://azure.microsoft.com/overview/azure-stack/) é uma plataforma de nuvem híbrida que permite que você forneça serviços Azure do seu data center. Azure Stack é gerenciado usando o PowerShell ou o portal do administrador, que é semelhante ao portal do Azure tradicional usado para acessar e gerenciar os serviços do Azure tradicionais. Windows Admin Center não foi elaborado para gerenciar a infraestrutura de pilha do Azure, mas você pode usá-lo para [Gerenciar as máquinas virtuais do Azure IaaS](../azure/manage-azure-vms.md) (executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012) ou solucionar problemas físicos individuais servidores implantados no ambiente Azure Stack.

## System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) é uma solução de gerenciamento do Centro de dados locais para implantação, configuração, gerenciamento, monitoramento de seu centro de dados inteiro. System Center permite que você veja o status de todos os sistemas em seu ambiente, enquanto o Windows Admin Center permite que você faça uma busca detalhada um servidor específico para gerenciar ou solução de problemas com ferramentas mais granulares.

| WindowsAdmin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Plataforma de "" nativas reimaginado & ferramentas** | **Monitoramento de & de gerenciamento do Datacenter** |
| Incluído com a licença do Windows Server – **sem custo adicional**, assim como o MMC e outras ferramentas de caixa de entrada tradicionais | Conjunto **abrangente** de soluções para o valor adicional em seu ambiente e plataformas |
| **Leve**, com base em navegador gerenciamento remoto de instâncias do Windows Server, **em qualquer lugar**; alternativa para RDP | Gerenciar & monitor **heterogêneos** sistemas **em escala**, incluindo o Hyper-V, VMware e Linux |
|**Profunda** & de servidor único cluster único aprofundamento de solução de problemas, a manutenção de & de configuração|Infraestrutura de provisionamento; automação e autoatendimento;  infraestrutura e monitoramento **amplitude** da carga de trabalho|
|Gerenciamento otimizado de clusters **HCI** nó **individual** 2 – 4, integração do Hyper-V, espaços de armazenamento diretos e SDN|Implantar & gerenciar o Hyper-V, clusters de servidores do Windows em **escala de datacenter** de **bare-metal** com SCVMM|
|Somente o **monitoramento no HCI** ; serviço de integridade do cluster armazena histórico. Plataforma extensível para terceiros 1º e 3ª **extensões de ferramenta de administração**|**Extensible** & plataforma**escalável monitoramento** no SCOM, com alerta, notificações, carga de trabalho de terceiros monitoramento; SQL para histórico|
|Ponte mais fácil para **híbrido**; integrar e usar uma variedade de serviços do Azure para proteção de dados, replicação, atualizações e muito mais|Proteção de dados **internas** , replicação, atualizações (VMM/DPM/SCCM). Integração de híbrido com Log Analytics e o serviço de mapa|
|**Incremente os recursos de plataforma** do Windows Server: serviço de migração do armazenamento, a réplica de armazenamento, Insights do sistema, etc.|**Plataformas adicionais**: automação em Orchestrator/SMA. Integração com SCSM & outras ferramentas de gerenciamento de serviço|

#### Cada agregue valor direcionada de forma independente; **melhor juntos** com recursos complementares.
