---
title: Microsoft Hyper-V Server 2016
ms.prod: windows-server
ms.technology: compute-hyper-v
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: kbdazure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: f5c68f260ff90a07a17a39fdbb881ddcbdb857ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853259"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

O Microsoft Hyper-V Server 2016 é um produto autônomo\-que contém apenas o hipervisor do Windows, um modelo de driver do Windows Server e componentes de virtualização. Ele fornece uma solução de virtualização simples e confiável para ajudá-lo a melhorar a utilização do servidor e reduzir os custos.

A tecnologia de hipervisor do Windows no Microsoft Hyper-V Server 2016 é a mesma do que a função Hyper\-V no Windows Server 2016. Portanto, grande parte do conteúdo disponível para a função Hyper\-V no Windows Server 2016 também se aplica ao Microsoft Hyper-V Server 2016.

## <a name="hyper-v-server-resources-for-it-pros"></a>Recursos do Hyper\-V Server para profissionais de ti

|Tarefas|Recursos|
|-|-|
|![Símbolo de atende aos requisitos](media/All_Symbols_MeetsRequirements.png)|**Avaliar o Hyper-V**<p>[visão geral da tecnologia Hyper-V](hyper-v-technology-overview.md) -   <br />- [o que há de novo no Hyper-V no Windows Server 2016](what-s-new-in-hyper-v-on-windows.md)<br />-   [requisitos de sistema para o Hyper-V no Windows Server 2016](system-requirements-for-hyper-v-on-windows.md)<br />-   [sistemas operacionais convidados do Windows com suporte para o Hyper-V](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [máquinas virtuais Linux e FreeBSD com suporte](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />[compatibilidade de recursos de -   por geração e convidado](hyper-v-feature-compatibility-by-generation-and-guest.md)<p>**Planejar o Hyper-V**<p>-Decida qual [geração de máquina virtual](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) atende às suas necessidades. <br/>-Se você estiver movendo ou importando máquinas virtuais, decida quando [atualizar a versão](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md). <br />[escalabilidade](plan/plan-hyper-v-scalability-in-windows-server.md) de -  <br />- [rede](plan/plan-hyper-v-networking-in-windows-server.md) <br />[segurança](plan/plan-hyper-v-security-in-windows-server.md) de - |
|![Símbolo de introdução](media/All_Symbols_GetStarted.png)|**Introdução ao Hyper-V Server**<p>[Baixe e instale o Microsoft Hyper\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016). Isso instala o hipervisor do Windows, um modelo de driver do Windows Server e componentes de virtualização. É semelhante a executar a opção de instalação Server Core do Windows Server 2016 e da função Hyper\-V.|
|![Gerenciar símbolo](media/All_Symbols_Administrator.png)|**Configurar e gerenciar o servidor Hyper-V**<p>O Hyper\-V Server não tem uma interface gráfica do usuário \(\)GUI. Você pode usar as ferramentas a seguir para configurar e gerenciar o Hyper\-V Server.<p>-   [Configurar uma instalação Server Core do Windows server 2016 com sconfig. cmd](../../get-started/sconfig-on-ws2016.md) para atualizar as configurações de domínio ou grupo de trabalho, alterar Windows Update configurações, habilitar o gerenciamento remoto e muito mais.<br />-Use um [prompt de comando](../../administration/windows-commands/windows-commands.md) comum para comandos não disponíveis no sconfig.<br />-Use o [hyper\-v Manager](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management) ou o [Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm) para gerenciar remotamente o Hyper\-v Server. Para usar o Hyper\-V Manager, [Instale a função hyper\-v no Windows 10 ou no](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) [Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md).<br />-Consulte [instalar o Server Core](../../get-started/getting-started-with-server-core.md) para obter opções de gerenciamento adicionais para funções básicas do servidor que não são específicas do Hyper\-V. A maioria dos métodos de gerenciamento documentados também funcionam com o Hyper\-V Server.<p>**Configurar e gerenciar máquinas virtuais do Hyper\-V**<p>-   [criar um comutador virtual para máquinas virtuais do Hyper-V](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [criar uma máquina virtual no Hyper-V](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [escolher entre pontos de verificação padrão ou de produção](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [habilitar ou desabilitar pontos de verificação](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [gerenciar máquinas virtuais do Windows com o PowerShell Direct](manage/manage-windows-virtual-machines-with-powershell-direct.md) <p>**Implantar**<p>-   [Configurar hosts para migração ao vivo sem clustering de failover](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [Atualizar nós de cluster do Windows Server](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />[versão da máquina virtual de atualização](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md) de - <br />|
