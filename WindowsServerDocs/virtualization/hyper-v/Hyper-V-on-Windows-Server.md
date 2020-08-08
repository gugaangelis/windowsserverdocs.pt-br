---
title: Hyper-V no Windows Server
description: Fornece links para artigos importantes sobre como experimentar, planejar, implantar e gerenciar o Hyper-V
manager: dongill
ms.topic: article
ms.assetid: 0baef6b8-598c-4fe0-9f31-5869fc4e0f69
author: kbdazure
ms.author: kathydav
ms.date: 10/07/2016
ms.openlocfilehash: 61ed737c72e65fa040e09c546998542734457e89
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960809"
---
# <a name="hyper-v-on-windows-server"></a>Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2019

A função Hyper-V no Windows Server permite criar um ambiente de computação virtualizado no qual você pode criar e gerenciar máquinas virtuais. Você pode executar vários sistemas operacionais em um computador físico e isolar os sistemas operacionais uns dos outros. Com essa tecnologia, você pode melhorar a eficiência dos seus recursos de computação e liberar seus recursos de hardware.

Consulte os tópicos na tabela a seguir para saber mais sobre o Hyper-V no Windows Server.

## <a name="hyper-v-resources-for-it-pros"></a>Recursos do Hyper-V para profissionais de ti

|Tarefa |Recursos|
|---|---|
|![A marca de seleção e o ícone do documento para mostrar os requisitos são atendidos](media/All_Symbols_MeetsRequirements.png)|**Avaliar o Hyper-V**<p>- [Visão geral da tecnologia Hyper-V](Hyper-V-Technology-Overview.md)<br />- [O que há de novo no Hyper-V no Windows Server](What-s-new-in-Hyper-V-on-Windows.md)<br />- [Requisitos do sistema para o Hyper-V no Windows Server](System-requirements-for-Hyper-V-on-Windows.md)<br />- [Sistemas operacionais convidados do Windows com suporte para Hyper-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md) <br />- [Máquinas virtuais Linux e FreeBSD com suporte](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)<br />- [Compatibilidade de recursos por geração e convidado](Hyper-V-feature-compatibility-by-generation-and-guest.md) <p>**Planejar o Hyper-V**<p>- [Devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md) <br />- [Planejar a escalabilidade do Hyper-V no Windows Server](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [Planejar a rede do Hyper-V no Windows Server](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [Planejar a segurança do Hyper-V no Windows Server](plan/plan-hyper-v-security-in-windows-server.md)|
|![Ícone de cursor e explosão solar](media/All_Symbols_GetStarted.png)|**Introdução ao Hyper-V**<p>- [Baixe e instale o Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)<p>**Opção de instalação Server Core ou GUI do Windows Server 2019 como host de máquina virtual**<p>- [Instalar a função Hyper-V no Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)<br />- [Criar um comutador virtual para máquinas virtuais do Hyper-V](get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)<br />- [Criar uma máquina virtual no Hyper-V](get-started/Create-a-virtual-machine-in-Hyper-V.md)|
|![Ícone de pessoa e ferramentas](media/All_Symbols_Administrator.png)|**Atualizar hosts e máquinas virtuais do Hyper-V**<p>- [Atualizar nós de cluster do Windows Server](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)<br />- [Atualizar versão da máquina virtual](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)<p>**Configurar e gerenciar o Hyper-V**<p>- [Configurar hosts para migração ao vivo sem clustering de failover](deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)<br />- [Gerenciando o nano Server remotamente](../../get-started/manage-nano-server.md)<br />- [Escolha entre pontos de verificação padrão ou de produção](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)<br />- [Habilitar ou desabilitar pontos de verificação](manage/Enable-or-disable-checkpoints-in-Hyper-V.md)<br />- [Gerenciar máquinas virtuais do Windows com o PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)<br />- [Configurar a réplica do Hyper-V](manage/Set-up-Hyper-V-Replica.md)|
|![Ícone de bolhas de conversa](media/All_Symbols_Chat.png)|**Blogs**<p>Confira as últimas postagens de gerentes de programa, gerentes de produto, desenvolvedores e testadores nas equipes de virtualização e Hyper-V da Microsoft.<p>- [Blog de virtualização](https://blogs.technet.com/b/virtualization/)<br />- [Blog do Windows Server](https://blogs.technet.com/b/windowsserver/)<br />- [Blog de virtualização de Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/) (arquivado)|
|![Ícone de grupo de usuários](media/All_Symbols_Users_Group.png)|**Fórum e grupos de notícias**<p>Tem dúvidas? Converse com seus colegas, MVPs e a equipe de produto do Hyper-V.<p>- [Comunidade do Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)<br />- [Fórum do Windows Server Hyper-V no TechNet](https://docs.microsoft.com/answers/topics/windows-server-hyper-v.html)|

## <a name="related-technologies"></a>Tecnologias relacionadas

A tabela a seguir lista as tecnologias que você pode querer usar em seu ambiente de computação de virtualização.

|Tecnologia|Descrição|
|--------------|---------------|
|[Hyper-V Cliente](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)|Tecnologia de virtualização incluída no Windows 8, Windows 8.1 e Windows 10 que você pode instalar por meio de **programas e recursos** no **painel de controle**.|
|[Clustering de failover](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering)|Recurso do Windows Server que fornece alta disponibilidade para hosts e máquinas virtuais do Hyper-V.|
|[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)|Componente do System Center que fornece uma solução de gerenciamento para o datacenter virtualizado. Você pode configurar e gerenciar seus hosts de virtualização, redes e recursos de armazenamento para que você possa criar e implantar máquinas virtuais e serviços em nuvens privadas que você criou.|
|[Contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/)|Use os contêineres do Windows Server e do Hyper-V para fornecer ambientes padronizados para equipes de desenvolvimento, teste e produção.|