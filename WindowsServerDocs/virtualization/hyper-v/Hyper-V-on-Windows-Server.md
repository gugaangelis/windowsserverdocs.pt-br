---
title: Hyper-V no Windows Server
description: Fornece links para artigos importantes sobre experimentando, planejamento, implantação e gerenciamento do Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0baef6b8-598c-4fe0-9f31-5869fc4e0f69
author: KBDAzure
ms.author: kathydav
ms.date: 10/07/2016
ms.openlocfilehash: 1a658b611b68d7ecde64bdf0f8a318cc1af2804c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880257"
---
# <a name="hyper-v-on-windows-server"></a>Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2019

A função Hyper-V no Windows Server permite que você criar um ambiente virtualizado no qual você pode criar e gerenciar máquinas virtuais. Você pode executar vários sistemas operacionais em um computador físico e isolar os sistemas operacionais uns dos outros. Com essa tecnologia, você pode melhorar a eficiência dos recursos de computação e liberar seus recursos de hardware.

Consulte os tópicos na tabela a seguir para saber mais sobre o Hyper-V no Windows Server.

## <a name="hyper-v-resources-for-it-pros"></a>Recursos do Hyper-V para os profissionais de TI

|Tarefa |Recursos|
|---|---|
|![Marca de seleção e o ícone do documento para mostrar os requisitos forem atendidos](media/All_Symbols_MeetsRequirements.png)|**Avalie o Hyper-V**<br /><br />- [Visão geral da tecnologia Hyper-V](Hyper-V-Technology-Overview.md)<br />- [O que há de novo no Hyper-V no Windows Server](What-s-new-in-Hyper-V-on-Windows.md)<br />- [Requisitos de sistema do Hyper-V no Windows Server](System-requirements-for-Hyper-V-on-Windows.md)<br />- [Sistemas de operacionais de convidados Windows com suporte para Hyper-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md) <br />- [Máquinas virtuais Linux e FreeBSD com suporte](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)<br />- [Compatibilidade do recurso por geração e convidado](Hyper-V-feature-compatibility-by-generation-and-guest.md) <br /><br />**Planejar para o Hyper-V**<br /><br />- [Deve criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md) <br />- [Planejar a escalabilidade do Hyper-V no Windows Server](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [Planejar a rede do Hyper-V no Windows Server](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [Planejar a segurança do Hyper-V no Windows Server](plan/plan-hyper-v-security-in-windows-server.md)|
|![Ícone de cursor e explosão solar](media/All_Symbols_GetStarted.png)|**Introdução ao Hyper-V**<br /><br />- [Baixe e instale o Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)<br /><br />**Opção de instalação de Server Core ou interface gráfica do usuário a do Windows Server 2019 como host de máquina virtual**<br /><br />- [Instalar a função Hyper-V no Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)<br />- [Criar um comutador virtual para máquinas virtuais Hyper-V](get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)<br />- [Criar uma máquina virtual no Hyper-V](get-started/Create-a-virtual-machine-in-Hyper-V.md)|
|![Ícone de pessoa e ferramentas](media/All_Symbols_Administrator.png)|**Atualizar hosts do Hyper-V e máquinas virtuais**<br /><br />- [Atualizar nós de cluster do Windows Server](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)<br />- [Atualizar a versão da máquina virtual](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)<br /><br />**Configurar e gerenciar o Hyper-V**<br /><br />- [Configurar hosts para migração ao vivo sem Clustering de Failover](deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)<br />- [Gerenciar remotamente o Nano Server](../../get-started/manage-nano-server.md)<br />- [Escolha entre pontos de verificação padrão ou de produção](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)<br />- [Habilitar ou desabilitar os pontos de verificação](manage/Enable-or-disable-checkpoints-in-Hyper-V.md)<br />- [Gerenciar máquinas de virtuais do Windows com o PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)<br />- [Configurar a réplica do Hyper-V](manage/Set-up-Hyper-V-Replica.md)|
|![Ícone de bolhas de conversa](media/All_Symbols_Chat.png)|**Blogs**<br /><br />Confira as postagens mais recentes de gerentes de programa, gerentes de produto, os desenvolvedores e testadores em equipes do Microsoft Virtualization e Hyper-V.<br /><br />- [Blog de virtualização](https://blogs.technet.com/b/virtualization/)<br />- [Blog do Windows Server](https://blogs.technet.com/b/windowsserver/)<br />- [Blog de virtualização Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/) (arquivados)|
|![Ícone do grupo de usuário](media/All_Symbols_Users_Group.png)|**Fórum e grupos de notícias**<br /><br />Você tem perguntas? Fale com seus colegas, MVPs e a equipe do produto Hyper-V.<br /><br />- [Comunidade do Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)<br />- [Fórum TechNet do Windows Server Hyper-V](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverhyperv)|

## <a name="related-technologies"></a>Tecnologias relacionadas

A tabela a seguir lista as tecnologias que você talvez queira usar em seu ambiente de computação de virtualização.

|Tecnologia|Descrição|
|--------------|---------------|
|[Hyper-V cliente](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)|Incluído com o Windows 8, Windows 8.1 e Windows 10 que você pode instalar por meio de tecnologia de virtualização **programas e recursos** na **painel de controle**.|
|[Clustering de failover](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering)|Recurso do Windows Server que fornece alta disponibilidade para máquinas virtuais e hosts Hyper-V.|
|[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)|Componente do System Center que fornece uma solução de gerenciamento para o datacenter virtualizado. Você pode configurar e gerenciar seus hosts de virtualização, redes e recursos de armazenamento, portanto, você pode criar e implantar máquinas virtuais e serviços em nuvens privadas que você criou.|
|[Contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/)|Use contêineres do Windows Server e do Hyper-V para fornecer ambientes padronizados para equipes de desenvolvimento, teste e produção.|