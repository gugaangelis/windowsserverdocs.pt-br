---
title: Microsoft Hyper-V Server 2016
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: KBDAzure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: 9a1b6ea7b9abc94f63a1390b6fa18e4c8d4a1822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839367"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft Hyper-V Server 2016 é um suporte\-produto autônomo que contém somente o hipervisor do Windows, um modelo de driver do Windows Server e componentes de virtualização. Ele fornece uma solução de virtualização simples e confiável para ajudá-lo a melhorar a utilização do servidor e reduzir os custos.

A tecnologia de hipervisor do Windows no Microsoft Hyper-V Server 2016 é o mesmo que o que está no Hyper\-função V no Windows Server 2016. Dessa forma, grande parte do conteúdo disponível para o Hyper\-função V no Windows Server 2016 também se aplica ao Microsoft Hyper-V Server 2016.

## <a name="hyper-v-server-resources-for-it-pros"></a>Hyper\-recursos V Server para profissionais de TI

|Tarefas|Recursos|
|-|-|
|![Símbolo do atende aos requisitos](media/All_Symbols_MeetsRequirements.png)|**Avalie o Hyper-V**<br /><br />-   [Visão geral da tecnologia Hyper-V](hyper-v-technology-overview.md)<br />- [O que há de novo no Hyper-V no Windows Server 2016](what-s-new-in-hyper-v-on-windows.md)<br />-   [Requisitos de sistema do Hyper-V no Windows Server 2016](system-requirements-for-hyper-v-on-windows.md)<br />-   [Sistemas de operacionais de convidados Windows com suporte para Hyper-V](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [Linux com suporte e máquinas virtuais FreeBSD](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />-   [Compatibilidade do recurso por geração e convidado](hyper-v-feature-compatibility-by-generation-and-guest.md)<br /><br />**Planejar para o Hyper-V**<br /><br />– Decida quais [geração de máquina virtual](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) atende às suas necessidades. <br/>– Se você estiver movendo ou importando máquinas virtuais, decidir quando [atualizar a versão](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md). <br />- [Escalabilidade](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [Sistema de rede](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [Segurança](plan/plan-hyper-v-security-in-windows-server.md)|
|![Obter o símbolo de Introdução](media/All_Symbols_GetStarted.png)|**Introdução ao Hyper-V Server**<br /><br />[Baixe e instale o Microsoft Hyper\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016). Isso instala o hipervisor do Windows, um modelo de driver do Windows Server e componentes de virtualização. Ele é semelhante a executar a opção de instalação Server Core do Windows Server 2016 e o Hyper\-função V.|
|![Gerenciar o símbolo](media/All_Symbols_Administrator.png)|**Configurar e gerenciar o Hyper-V Server**<br /><br />Hyper\-V Server não tem uma interface de usuário gráfica \(GUI\). Você pode usar as ferramentas a seguir para configurar e gerenciar o Hyper\-V Server.<br /><br />-   [Configurar uma instalação Server Core do Windows Server 2016 com sconfig. cmd](../../get-started/sconfig-on-ws2016.md) para atualizar as configurações de domínio ou grupo de trabalho, altere o Windows atualizar as configurações, habilitar o gerenciamento remoto e muito mais.<br />– Use um comum [prompt de comando](../../administration/windows-commands/windows-commands.md) para comandos não está disponíveis no Sconfig.<br />-Use [Hyper\-Manager V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management) ou [do Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm) para gerenciar remotamente Hyper\-V Server. Para usar o Hyper\-Gerenciador de V [instalar o Hyper\-função V no Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) ou [Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md).<br />-Consulte [instalar o Server Core](../../get-started/getting-started-with-server-core.md) para obter opções de gerenciamento adicional para as funções de servidor básico que não são específicas de Hyper\-V. A maioria dos métodos de gerenciamento documentadas aqui também funcionam com Hyper\-V Server.<br /><br />**Configurar e gerenciar o Hyper\-máquinas virtuais V**<br /><br />-   [Criar um comutador virtual para máquinas virtuais Hyper-V](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [Criar uma máquina virtual no Hyper-V](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [Escolha entre pontos de verificação padrão ou de produção](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [Habilitar ou desabilitar os pontos de verificação](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [Gerenciar máquinas de virtuais do Windows com o PowerShell Direct](manage/manage-windows-virtual-machines-with-powershell-direct.md) <br /><br />**Implantar**<br /><br />-   [Configurar hosts para migração ao vivo sem Clustering de Failover](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [Atualizar nós de cluster do Windows Server](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [Atualizar a versão da máquina virtual](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
