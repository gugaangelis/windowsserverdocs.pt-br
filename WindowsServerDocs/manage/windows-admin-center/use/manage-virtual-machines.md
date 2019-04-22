---
title: Gerenciando máquinas virtuais com Windows Admin Center
description: Gerenciando hosts Hyper-V e máquinas virtuais com Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 41767b9e53c0106931e78f86f8675e413cca0d0a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816877"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>Gerenciando máquinas virtuais com Windows Admin Center

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

A ferramenta de máquinas virtuais está disponível no [Server](manage-servers.md), [Cluster de Failover](manage-failover-clusters.md) ou [Hyper-Converged Cluster](manage-hyper-converged.md) conexões se a função Hyper-V estiver habilitada no servidor ou cluster. Você pode usar a ferramenta de máquinas virtuais para gerenciar hosts Hyper-V executando o Windows Server 2012 ou posterior, seja instalado com a experiência de área de trabalho ou como núcleo de servidor. Também há suporte para Hyper-V Server 2012 e 2016.

## <a name="key-features"></a>Principais recursos

Destaques da ferramenta de máquinas virtuais no Windows Admin Center incluem:

- **Hyper-V host recurso monitoramento de alto nível.** Exiba uso da CPU e memória geral, as métricas de desempenho de e/s, alertas de integridade da VM e eventos para o servidor host Hyper-V ou cluster inteiro em um único painel.
- **Experiência unificada reunindo os recursos do Gerenciador do Hyper-V e o Gerenciador de Cluster de Failover.** Exibir todas as máquinas virtuais em um cluster e fazer uma busca detalhada em uma única máquina virtual para o gerenciamento avançado e solução de problemas.
- **Fluxos de trabalho simplificados, mas poderosos para gerenciamento de máquinas virtuais.** Novas experiências de interface do usuário projetadas para cenários de administração de TI criar, gerenciar e replicar máquinas virtuais.

Aqui estão algumas das tarefas do Hyper-V, que você pode fazer no Windows Admin Center:

- [Monitorar o desempenho e os recursos de host do Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Exibir o inventário de máquina virtual](#view-virtual-machine-inventory)
- [Criar uma nova máquina virtual](#create-a-new-virtual-machine)
- [Alterar as configurações de máquina virtual](#change-virtual-machine-settings)
- [Migração ao vivo de uma máquina virtual para outro nó do cluster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Gerenciamento avançado e solução de problemas de uma única máquina virtual](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Exibir logs de eventos do Hyper-V](#view-hyper-v-event-logs)
- [Proteger as máquinas virtuais com o Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>Monitorar o desempenho e os recursos de host do Hyper-V

![Tela de resumo de máquinas virtuais](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Clique o **máquinas virtuais** ferramenta do painel de navegação do lado esquerdo.
2. Há duas guias na parte superior a **máquinas virtuais** ferramenta, o **resumo** guia e o **inventário** guia. O **resumo** guia fornece uma visão holística de recursos de host do Hyper-V e desempenho para o servidor atual ou o cluster inteiro, incluindo o seguinte:
    - O número de VMs, agrupados por estado – em execução, off, em pausa e salvo
    - Alertas de integridade recente ou eventos de log de eventos do Hyper-V (alertas só estão disponíveis para clusters hiperconvergente que executa o Windows Server 2016 ou posterior)
    - Uso de CPU e memória com divisão do host vs convidado
    - VMs principais consumo a maioria dos recursos de CPU e memória
    - Gráficos de taxa de transferência IOPS e/s de linhas de dados históricos e em tempo real (gráficos de linhas de desempenho de armazenamento só estão disponíveis para clusters hiperconvergente que executa o Windows Server 2016 ou posterior. Os dados históricos só estão disponíveis para clusters hiperconvergente executando o Windows Server 2019)

## <a name="view-virtual-machine-inventory"></a>Exibir o inventário de máquina virtual

![Tela de inventário de máquinas virtuais](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Clique o **máquinas virtuais** ferramenta do painel de navegação do lado esquerdo.
2. Há duas guias na parte superior a **máquinas virtuais** ferramenta, o **resumo** guia e o **inventário** guia. O **inventário** guia lista as máquinas virtuais disponíveis no servidor atual ou todo o cluster e fornece comandos para gerenciar máquinas virtuais individuais. Você pode:
    - Exiba uma lista das máquinas virtuais em execução no servidor atual ou no cluster.
    - Exiba o servidor de host e o estado da máquina virtual se você estiver exibindo as máquinas virtuais para um cluster. Exiba também o uso de CPU e memória da perspectiva do host, incluindo a pressão de memória, a demanda por memória e memória atribuída e da máquina virtual de tempo de atividade, pulsação e status de proteção usando o Azure Site Recovery.
    - [Criar uma nova máquina virtual](#create-a-new-virtual-machine).
    - Excluir, iniciar, desligar, desligar, pausar, retomar, redefinir ou renomear uma máquina virtual. Também salvar a máquina virtual, excluir um estado salvo ou criar um ponto de verificação.
    - [Alterar as configurações para uma máquina virtual](#change-virtual-machine-settings).
    - Conecte-se a um console de máquina virtual usando o VMConnect por meio do host Hyper-V.
    - [Replicar uma máquina virtual usando o Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

OBSERVAÇÃO: Se você estiver conectado a um cluster, a ferramenta de máquina Virtual exibirá apenas as máquinas virtuais em cluster. Planejamos também mostram a máquinas virtuais não clusterizadas no futuro.

## <a name="create-a-new-virtual-machine"></a>Criar uma nova máquina virtual

![Criar nova tela de máquina virtual](../media/manage-virtual-machines/new-vm.png)

1. Clique o **máquinas virtuais** ferramenta do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta de máquinas virtuais, escolha o **estoque** guia e, em seguida, clique em **New** para criar uma nova máquina virtual.
3. Insira o nome da máquina virtual e escolha entre as máquinas virtuais de geração 1 e 2.
4. Se você estiver criando uma máquina virtual em um cluster, você pode escolher para criar inicialmente a máquina virtual na qual host. Se você estiver executando o Windows Server 2016 ou posterior, a ferramenta fornecerá uma recomendação de host para você.
5. Escolha um caminho para os arquivos de máquina virtual. Escolha um volume na lista suspensa ou clique em **procurar** para escolher uma pasta usando o seletor de pasta. Os arquivos de configuração de máquina virtual e o arquivo de disco rígido virtual serão salvo em uma única pasta sob o `\Hyper-V\\[virtual machine name]` caminho do volume selecionado ou caminho.
6. Escolha o número de processadores virtuais, se você virtualização aninhada habilitada, configure as configurações de memória, adaptadores de rede, discos rígidos virtuais e escolher se deseja instalar um sistema operacional de um arquivo de imagem. ISO ou de rede.
7. Clique em **criar** para criar a máquina virtual.
8. Depois que a máquina virtual é criada e aparece na lista de máquinas virtuais, você pode iniciar a máquina virtual.
9. Depois que a máquina virtual é iniciada, você pode se conectar ao console da máquina virtual por meio do VMConnect para instalar o sistema operacional. Selecione a máquina virtual na lista, clique em **mais** > **Connect** para baixar o arquivo. rdp. Abra o arquivo. RDP no aplicativo da Conexão de área de trabalho remota. Como isso está se conectando ao console da máquina virtual, você precisará inserir credenciais de administrador do host do Hyper-V.

## <a name="change-virtual-machine-settings"></a>Alterar as configurações de máquina virtual

![Tela de configurações de máquina virtual](../media/manage-virtual-machines/vm-settings.png)

1. Clique o **máquinas virtuais** ferramenta do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta de máquinas virtuais, escolha o **inventário** guia. Escolha uma máquina virtual na lista e clique em **mais** > **configurações**.
3. Alternar entre o **gerais**, **memória**, **processadores**, **discos**, **redes**, **Ordem de inicialização** e **pontos de verificação** guia, defina as configurações necessárias e clique em **salvar** para salvar as configurações da guia atual. As configurações disponíveis variam dependendo da geração da máquina virtual. Além disso, algumas configurações não podem ser alteradas para máquinas virtuais em execução e você precisará interromper a máquina virtual pela primeira vez.

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>Migração ao vivo de uma máquina virtual para outro nó do cluster

Se você estiver conectado a um cluster, você pode migrar ao vivo uma máquina virtual para outro nó de cluster.

1. De um Cluster de Failover ou a conexão de cluster hiperconvergente, clique no **máquinas virtuais** ferramenta do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta de máquinas virtuais, escolha o **inventário** guia. Escolha uma máquina virtual na lista e clique em **mais** > **mover**.
3. Escolha um servidor na lista de nós de cluster disponíveis e clique em **mover**.
4. Notificações para ver o andamento de movimentação serão exibidas no canto superior direito de Windows Admin Center. Se a movimentação for bem-sucedida, você verá o nome do servidor Host alterado na lista de máquina virtual.

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>Gerenciamento avançado e solução de problemas de uma única máquina virtual

![Tela de detalhes da máquina virtual única](../media/manage-virtual-machines/vm-details.png)

Você pode exibir informações detalhadas e gráficos de desempenho para uma única máquina virtual da página de única máquina virtual.

1. Clique o **máquinas virtuais** ferramenta do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta de máquinas virtuais, escolha o **inventário** guia. Clique no nome de uma máquina virtual da lista de máquinas virtuais.
3. Na página única máquina virtual, você pode:
    - Exiba informações detalhadas para a máquina virtual.
    - Exibir gráficos de linhas de dados históricos e ao vivo para CPU, memória, rede e taxa de transferência IOPS e/s (dados históricos só estão disponíveis para clusters hiperconvergente executando o Windows Server 2019)
    - Exibir, criar, aplicar, renomear e excluir pontos de verificação.
    - Exibir detalhes de arquivos de disco rígido virtual (. vhd) da máquina virtual, adaptadores de rede e servidor host.
    - Excluir, iniciar, desligar, desligar, pausar, retomar, redefinir ou renomear a máquina virtual. Também salvar a máquina virtual, excluir um estado salvo ou criar um ponto de verificação.
    - [Alterar as configurações para a máquina virtual](#change-virtual-machine-settings).
    - Conecte-se para o console de máquina virtual usando o VMConnect por meio do host Hyper-V.
    - [Replique a máquina virtual usando o Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## <a name="view-hyper-v-event-logs"></a>Exibir logs de eventos do Hyper-V

Você pode exibir os logs de eventos do Hyper-V diretamente da ferramenta de máquinas virtuais.

1. Clique o **máquinas virtuais** ferramenta do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta de máquinas virtuais, escolha o **resumo** guia. Na seção superior direita eventos, clique em **todos os eventos de exibição**.
3. A ferramenta de Visualizador de eventos mostrará os canais de evento do Hyper-V no painel esquerdo. Escolha um canal para exibir os eventos no painel direito. Se você estiver gerenciando um cluster de failover ou um cluster hiperconvergente, os logs de eventos exibirá eventos para todos os nós de cluster, exibindo o servidor host na coluna de máquina.

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>Proteger as máquinas virtuais com o Azure Site Recovery

Você pode usar o Windows Admin Center para configurar o Azure Site Recovery e replicar suas máquinas virtuais de locais para o Azure. [Saiba mais](azure-services.md)

## <a name="more-coming"></a>Aguarde mais

Gerenciamento de máquinas virtuais no Windows Admin Center ativamente está em desenvolvimento e novos recursos serão adicionados no futuro próximo. Você pode exibir o status e votar em recursos no UserVoice:

|Solicitação de recurso|
|-------|
|[Máquinas virtuais de importação/exportação](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[Máquinas virtuais de classificação em pastas](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[Configurações de máquina virtual adicional de suporte](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[Suporte de réplica do Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[Delegar a propriedade da máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[Clonar a máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[Criar um modelo de uma máquina virtual existente](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[Exibir máquinas de virtuais nos hosts Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[Configurar a VLAN no painel de nova máquina Virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**Ver todos os ou propor o novo recurso**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
