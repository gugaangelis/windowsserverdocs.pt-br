---
title: Gerenciando máquinas virtuais com o centro de administração do Windows
description: Gerenciando hosts Hyper-V e máquinas virtuais com o centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 02a33383b466e8bade2db0bbddaff66f0196954c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356861"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>Gerenciando máquinas virtuais com o centro de administração do Windows

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

A ferramenta de máquinas virtuais está disponível no [servidor](manage-servers.md), [cluster de failover](manage-failover-clusters.md) ou conexões de [cluster hiperconvergentes](manage-hyper-converged.md) se a função Hyper-V estiver habilitada no servidor ou cluster. Você pode usar a ferramenta de máquinas virtuais para gerenciar hosts Hyper-V que executam o Windows Server 2012 ou posterior, seja instalado com a experiência desktop ou como Server Core. Também há suporte para o Hyper-V Server 2012, 2016 e 2019.

## <a name="key-features"></a>Principais recursos

Os destaques da ferramenta de máquinas virtuais no centro de administração do Windows incluem:

- **Monitoramento de recursos de host Hyper-V de alto nível.** Exiba o uso geral de CPU e memória, as métricas de desempenho de e/s, os alertas de integridade da VM e os eventos para o servidor host Hyper-V ou o cluster inteiro em um único painel.
- **A experiência unificada que traz os recursos do Hyper-V Manager e Gerenciador de Cluster de Failover juntos.** Exiba todas as máquinas virtuais em um cluster e faça uma busca detalhada em uma única máquina virtual para obter gerenciamento e solução de problemas avançados.
- **Fluxos de trabalho simplificados, mas poderosos para gerenciamento de máquinas virtuais.** Novas experiências de interface do usuário adaptadas aos cenários de administração de ti para criar, gerenciar e replicar máquinas virtuais.

Aqui estão algumas das tarefas do Hyper-V que você pode fazer no centro de administração do Windows:

- [Monitorar recursos e desempenho do host Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Exibir inventário de máquina virtual](#view-virtual-machine-inventory)
- [Criar uma nova máquina virtual](#create-a-new-virtual-machine)
- [Alterar configurações de máquina virtual](#change-virtual-machine-settings)
- [Migrar ao vivo uma máquina virtual para outro nó de cluster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Gerenciamento avançado e solução de problemas para uma única máquina virtual](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Gerenciar uma máquina virtual por meio do host do Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Alterar as configurações de host do Hyper-V](#change-hyper-v-host-settings)
- [Exibir logs de eventos do Hyper-V](#view-hyper-v-event-logs)
- [Proteger máquinas virtuais com Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>Monitorar recursos e desempenho do host Hyper-V

![Tela de Resumo de máquinas virtuais](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Clique na ferramenta **máquinas virtuais** no painel de navegação do lado esquerdo.
2. Há duas guias na parte superior da ferramenta **máquinas virtuais** , a guia **Resumo** e a guia **inventário** . A guia **Resumo** fornece uma exibição holística dos recursos e do desempenho do host Hyper-V para o servidor atual ou o cluster inteiro, incluindo o seguinte:
    - O número de VMs agrupadas por estado-em execução, desativado, pausado e salvo
    - Alertas de integridade recentes ou eventos do log de eventos do Hyper-V (os alertas estão disponíveis somente para clusters hiperconvergentes que executam o Windows Server 2016 ou posterior)
    - Utilização de CPU e memória com host vs-divisão de convidado
    - Principais VMs que consomem a maioria dos recursos de CPU e memória
    - Gráficos de linha de dados dinâmicos e históricos para IOPS e taxa de transferência de e/s (gráficos de linha de desempenho de armazenamento só estão disponíveis para clusters hiperconvergentes que executam o Windows Server 2016 ou posterior. Os dados históricos só estão disponíveis para clusters hiperconvergentes que executam o Windows Server 2019)

## <a name="view-virtual-machine-inventory"></a>Exibir inventário de máquina virtual

![Tela de inventário de máquinas virtuais](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Clique na ferramenta **máquinas virtuais** no painel de navegação do lado esquerdo.
2. Há duas guias na parte superior da ferramenta **máquinas virtuais** , a guia **Resumo** e a guia **inventário** . A guia **inventário** lista as máquinas virtuais disponíveis no servidor atual ou no cluster inteiro e fornece comandos para gerenciar máquinas virtuais individuais. Você pode:
    - Exiba uma lista das máquinas virtuais em execução no servidor ou cluster atual.
    - Exiba o estado da máquina virtual e o servidor host se você estiver exibindo máquinas virtuais para um cluster. Exiba também a utilização de CPU e memória do ponto de vista do host, incluindo pressão de memória, demanda de memória e memória atribuída, além do tempo de atividade da máquina virtual, status de pulsação e status de proteção usando Azure Site Recovery.
    - [Crie uma nova máquina virtual](#create-a-new-virtual-machine).
    - Excluir, iniciar, desligar, desligar, pausar, retomar, redefinir ou renomear uma máquina virtual. Além disso, salve a máquina virtual, exclua um estado salvo ou crie um ponto de verificação.
    - [Alterar as configurações de uma máquina virtual](#change-virtual-machine-settings).
    - Conecte-se a um console de máquina virtual usando o VMConnect por meio do host Hyper-V.
    - [Replique uma máquina virtual usando Azure site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - Para operações que podem ser executadas em várias VMs, como iniciar, desligar, salvar, pausar, excluir, redefinir, você pode selecionar várias VMs e executar a operação de uma vez.

OBSERVAÇÃO:  Se você estiver conectado a um cluster, a ferramenta de máquina virtual exibirá apenas máquinas virtuais clusterizadas. Planejamos também mostrar máquinas virtuais não clusterizadas no futuro.

## <a name="create-a-new-virtual-machine"></a>Criar uma nova máquina virtual

![Tela criar nova máquina virtual](../media/manage-virtual-machines/new-vm.png)

1. Clique na ferramenta **máquinas virtuais** no painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** e clique em **novo** para criar uma nova máquina virtual.
3. Insira o nome da máquina virtual e escolha entre as máquinas virtuais de geração 1 e 2.
4. Se você estiver criando uma máquina virtual em um cluster, poderá escolher em qual host a máquina virtual será inicialmente criada. Se você estiver executando o Windows Server 2016 ou posterior, a ferramenta fornecerá uma recomendação de host para você.
5. Escolha um caminho para os arquivos de máquina virtual. Escolha um volume na lista suspensa ou clique em **procurar** para escolher uma pasta usando o seletor de pasta. Os arquivos de configuração de máquina virtual e o arquivo de disco rígido virtual serão salvos em uma única `\Hyper-V\\[virtual machine name]` pasta sob o caminho do volume ou caminho selecionado.

   >[!Tip]
   > No seletor de pasta, você pode navegar para qualquer compartilhamento SMB disponível na rede inserindo o caminho no campo **nome da pasta** como ```\\server\share```. Usar um compartilhamento de rede para o armazenamento de VM exigirá [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Escolha o número de processadores virtuais, se você deseja habilitar a virtualização aninhada, definir configurações de memória, adaptadores de rede, discos rígidos virtuais e escolher se deseja instalar um sistema operacional de um arquivo de imagem. ISO ou da rede.
7. Clique em **criar** para criar a máquina virtual.
8. Depois que a máquina virtual é criada e exibida na lista de máquinas virtuais, você pode iniciar a máquina virtual.
9. Depois que a máquina virtual for iniciada, você poderá se conectar ao console da máquina virtual por meio do VMConnect para instalar o sistema operacional. Selecione a máquina virtual na lista, clique em **mais** > **conexão** para baixar o arquivo. rdp. Abra o arquivo. rdp no aplicativo Conexão de Área de Trabalho Remota. Como isso está se conectando ao console da máquina virtual, será necessário inserir as credenciais de administrador do host Hyper-V.

## <a name="change-virtual-machine-settings"></a>Alterar configurações de máquina virtual

![Tela de configurações da máquina virtual](../media/manage-virtual-machines/vm-settings.png)

1. Clique na ferramenta **máquinas virtuais** no painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** . Escolha uma máquina virtual na lista e clique em **mais** **configurações** > .
3. Alterne entre a **guia Geral**, **segurança**, **memória**, **processadores**, **discos**, **redes**, **ordem de inicialização** e pontos de **verificação** , defina as configurações necessárias e clique em **salvar** para salvar as configurações da guia atual. As configurações disponíveis variam de acordo com a geração da máquina virtual. Além disso, algumas configurações não podem ser alteradas para máquinas virtuais em execução e você precisará parar a máquina virtual primeiro.

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>Migrar ao vivo uma máquina virtual para outro nó de cluster

Se você estiver conectado a um cluster, poderá migrar ao vivo uma máquina virtual para outro nó de cluster.

1. Em um cluster de failover ou conexão de cluster hiperconvergente, clique na ferramenta **máquinas virtuais** no painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** . Escolha uma máquina virtual na lista e clique em **mais** > **mover**.
3. Escolha um servidor na lista de nós de cluster disponíveis e clique em **mover**.
4. As notificações para o progresso da movimentação serão exibidas no canto superior direito do centro de administração do Windows. Se a movimentação for bem-sucedida, você verá o nome do servidor host alterado na lista de máquinas virtuais.

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>Gerenciamento avançado e solução de problemas para uma única máquina virtual

![Tela de detalhes da máquina virtual única](../media/manage-virtual-machines/vm-details.png)

Você pode exibir informações detalhadas e gráficos de desempenho para uma única máquina virtual a partir da página de máquina virtual única.

1. Clique na ferramenta **máquinas virtuais** no painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** . Clique no nome de uma máquina virtual na lista de máquinas virtuais.
3. Na página única máquina virtual, você pode:
    - Exiba informações detalhadas para a máquina virtual.
    - Exibir gráficos de linhas de dados dinâmicos e históricos para CPU, memória, rede, IOPS e taxa de transferência de e/s (dados históricos estão disponíveis somente para clusters hiperconvergentes que executam o Windows Server 2019)
    - Exibir, criar, aplicar, renomear e excluir pontos de verificação.
    - Exiba detalhes dos arquivos de disco rígido virtual (. vhd) da máquina virtual, adaptadores de rede e servidor host.
    - Excluir, iniciar, desligar, desligar, pausar, retomar, redefinir ou renomear a máquina virtual. Além disso, salve a máquina virtual, exclua um estado salvo ou crie um ponto de verificação.
    - [Altere as configurações da máquina virtual](#change-virtual-machine-settings).
    - Conecte-se ao console de máquina virtual usando o VMConnect por meio do host Hyper-V.
    - [Replique a máquina virtual usando Azure site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## <a name="manage-a-virtual-machine-through-the-hyper-v-host-vmconnect"></a>Gerenciar uma máquina virtual por meio do host do Hyper-V (VMConnect)

![Conexão de VM por meio do navegador da Web](../media/manage-virtual-machines/vm-connect.png)

1. Clique na ferramenta **máquinas virtuais** no painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** . Escolha uma máquina virtual na lista e clique em **mais** > **conectar** ou **baixar o arquivo RDP**. O **Connect** permitirá que você interaja com a VM convidada por meio do console Web área de trabalho remota, integrado no centro de administração do Windows. O **download do arquivo RDP** baixará um arquivo. rdp que você pode abrir com o aplicativo conexão de área de trabalho remota (mstsc. exe). As duas opções usarão VMConnect para se conectar à VM convidada por meio do host Hyper-V e exigirão que você insira as credenciais de administrador para o servidor host Hyper-V.

## <a name="change-hyper-v-host-settings"></a>Alterar as configurações de host do Hyper-V

![Tela de configurações de host do Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. Em um servidor, cluster hiperconvergente ou conexão de cluster de failover, clique no menu **configurações** na parte inferior do painel de navegação do lado esquerdo.
2. Em um cluster ou servidor de host do Hyper-V, você verá um grupo de **configurações de host do Hyper-v** com as seguintes seções:
    - Genéricos Alterar o caminho do arquivo de máquinas virtuais e discos rígidos virtuais e o tipo de agendamento do hipervisor (se houver suporte)
    - Modo de sessão avançado
    - Abrangência NUMA
    - Migração ao vivo
    - Migração de armazenamento
3. Se você fizer alterações de configuração de host Hyper-V em um cluster hiperconvergente ou conexão de cluster de failover, a alteração será aplicada a todos os nós de cluster.

## <a name="view-hyper-v-event-logs"></a>Exibir logs de eventos do Hyper-V

Você pode exibir logs de eventos do Hyper-V diretamente da ferramenta máquinas virtuais.

1. Clique na ferramenta **máquinas virtuais** no painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **Resumo** . Na seção eventos da parte superior direita, clique em **Exibir todos os eventos**.
3. A ferramenta Visualizador de Eventos mostrará os canais de eventos do Hyper-V no painel esquerdo. Escolha um canal para exibir os eventos no painel direito. Se você estiver gerenciando um cluster de failover ou um cluster hiperconvergente, os logs de eventos exibirão eventos para todos os nós de cluster, exibindo o servidor host na coluna computador.

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>Proteger máquinas virtuais com Azure Site Recovery

Você pode usar o centro de administração do Windows para configurar Azure Site Recovery e replicar suas máquinas virtuais locais para o Azure. [Saiba mais](../azure/azure-site-recovery.md)

## <a name="more-coming"></a>Mais em breve

O gerenciamento de máquinas virtuais no centro de administração do Windows está ativamente em desenvolvimento e novos recursos serão adicionados em breve. Você pode exibir o status e votar para recursos no UserVoice:

- [Importar/exportar máquinas virtuais](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)
- [Classificar máquinas virtuais em pastas](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)
- [Suporte a configurações de máquina virtual adicionais](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)
- [Suporte à réplica do Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)
- [Delegar a propriedade da máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)
- [Clonar máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)
- [Criar um modelo a partir de uma máquina virtual existente](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)
- [Exibir máquinas virtuais em hosts Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)
- [Configurar VLAN no painel nova máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)

[Veja todos ou proponha novos recursos](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D).
