---
title: Gerenciamento de máquinas virtuais com o Windows Admin Center
description: Gerenciar hosts Hyper-V e máquinas virtuais com o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 374ee30dcbaf9af3caa60ee85ec59fd3c158206b
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296748"
---
# Gerenciamento de máquinas virtuais com o Windows Admin Center

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

A ferramenta de máquinas virtuais está disponível em conexões de [servidor](manage-servers.md), [Cluster de Failover](manage-failover-clusters.md) ou [Cluster hiperconvergente](manage-hyper-converged.md) se a função Hyper-V está habilitada no servidor ou cluster. Você pode usar a ferramenta de máquinas virtuais para gerenciar hosts Hyper-V que executam o Windows Server 2012 ou posterior, seja instalado com experiência Desktop ou Server Core. Hyper-V Server 2012, 2016 e 2019 também têm suporte.

## Principais recursos

Destaques da ferramenta máquinas virtuais no Windows Admin Center:

- **Alto nível do Hyper-V host monitoração de recursos.** Exiba o uso de CPU e memória geral, as métricas de desempenho de e/s, alertas de integridade VM e eventos para o servidor de host do Hyper-V ou cluster inteiro em um único painel.
- **Experiência unificada reunindo recursos Gerenciador do Hyper-V e Gerenciador de Cluster de Failover.** Exibir todas as máquinas virtuais em um cluster e buscas detalhadas em uma única máquina virtual para gerenciamento avançado e solução de problemas.
- **Fluxos de trabalho simplificados, mas eficientes para o gerenciamento de máquina virtual.** Novas experiências de interface do usuário personalizados para cenários de administração de TI para criar, gerenciar e replicar as máquinas virtuais.

Aqui estão algumas das tarefas do Hyper-V, que você pode fazer no Windows Admin Center:

- [Monitorar o desempenho e recursos de host do Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Inventário de máquina virtual do modo de exibição](#view-virtual-machine-inventory)
- [Criar uma nova máquina virtual](#create-a-new-virtual-machine)
- [Alterar configurações de máquina virtual](#change-virtual-machine-settings)
- [Migração ao vivo uma máquina virtual para outro nó de cluster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Gerenciamento avançado e solução de problemas para uma única máquina virtual](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Gerenciar uma máquina virtual por meio do host do Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Alterar configurações de host do Hyper-V](#change-hyper-v-host-settings)
- [Exibir logs de eventos do Hyper-V](#view-hyper-v-event-logs)
- [Proteger máquinas virtuais com o Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## Monitorar o desempenho e recursos de host do Hyper-V

![Tela de resumo de máquinas virtuais](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Clique na ferramenta de **máquinas virtuais** do painel de navegação do lado esquerdo.
2. Há dois guias na parte superior da ferramenta de **máquinas virtuais** , na guia **Resumo** e a guia **inventário** . Guia **Resumo** fornece uma visão abrangente de recursos de host do Hyper-V e desempenho para o servidor atual ou o cluster inteiro, incluindo o seguinte:
    - O número de VMs agrupados pelo estado - em execução, desativado, pausado e salvo
    - Alertas de integridade recentes ou eventos de log de eventos do Hyper-V (alertas só estão disponíveis para clusters hiperconvergentes executando o Windows Server 2016 ou posterior)
    - Uso de CPU e memória com detalhamento do host vs convidado
    - VMs principais consome mais recursos de CPU e memória
    - Gráficos de taxa de transferência de IOPS e e/s de linha de dados ao vivo e históricos (gráficos de linha de desempenho de armazenamento só estão disponíveis para clusters hiperconvergentes executando o Windows Server 2016 ou posterior. Os dados históricos só estão disponíveis para clusters hiperconvergentes executando o Windows Server 2019)

## Inventário de máquina virtual do modo de exibição

![Tela de inventário de máquinas virtuais](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Clique na ferramenta de **máquinas virtuais** do painel de navegação do lado esquerdo.
2. Há dois guias na parte superior da ferramenta de **máquinas virtuais** , na guia **Resumo** e a guia **inventário** . Guia **estoque** lista as máquinas virtuais disponíveis no servidor atual ou todo o cluster e fornece comandos para gerenciar as máquinas virtuais individuais. Você pode:
    - Exiba uma lista das máquinas virtuais em execução no servidor atual ou cluster.
    - Exiba o servidor de host e o estado da máquina virtual se você estiver exibindo as máquinas virtuais para um cluster. Exiba o uso de CPU e memória da perspectiva do host, incluindo pressão de memória, demanda de memória e memória atribuída e a máquina virtual atividade, status de pulsação e status de proteção usando o Azure Site Recovery também.
    - [Criar uma nova máquina virtual](#create-a-new-virtual-machine).
    - Excluir, iniciar, desativar, desligar, pausar, retomar, redefinir ou renomear uma máquina virtual. Também salvar a máquina virtual, excluir um estado salvo ou criar um ponto de verificação.
    - [Alterar configurações para uma máquina virtual](#change-virtual-machine-settings).
    - Conecte a um console da máquina virtual usando o VMConnect por meio do host do Hyper-V.
    - [Replicar uma máquina virtual usando o Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - Para operações que podem ser executadas em várias VMs, como iniciar, desligar, salvar, pausar, excluir, redefinir, você pode selecionar várias VMs e executar a operação de uma só vez.

Observação: Se você estiver conectado a um cluster, a ferramenta de máquina Virtual só exibirá máquinas virtuais em cluster. Planejamos Mostrar também as máquinas virtuais sem cluster no futuro.

## Criar uma nova máquina virtual

![Criar nova tela de máquina virtual](../media/manage-virtual-machines/new-vm.png)

1. Clique na ferramenta de **máquinas virtuais** do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** , clique em **novo** para criar uma nova máquina virtual.
3. Insira o nome da máquina virtual e escolha entre máquinas virtuais de geração 1 e 2.
4. Se você estiver criando uma máquina virtual em um cluster, você pode escolher qual host inicialmente criar a máquina virtual no. Se você estiver executando o Windows Server 2016 ou posterior, a ferramenta fornecerá uma recomendação de host para você.
5. Escolha um caminho para os arquivos da máquina virtual. Escolha um volume na lista suspensa ou clique em **Procurar** para escolher uma pasta usando o seletor de pasta. O arquivo do disco rígido virtual e arquivos de configuração de máquina virtual serão salvos em uma única pasta sob o `\Hyper-V\\[virtual machine name]` caminho do volume selecionado ou caminho.

>[!Tip]
> O seletor de pasta, você pode navegar para qualquer compartilhamento SMB disponível na rede digitando o caminho no campo **nome da pasta** como ```\\server\share```. Usar um compartilhamento de rede para o armazenamento VM exigirá [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Escolha o número de processadores virtuais, se você deseja a virtualização aninhada habilitada, definir configurações de memória, adaptadores de rede, discos rígidos virtuais e escolha se deseja instalar um sistema operacional de um arquivo de imagem. ISO ou da rede.
7. Clique em **criar** para criar a máquina virtual.
8. Depois que a máquina virtual é criada e aparece na lista de máquina virtual, você pode iniciar a máquina virtual.
9. Depois que a máquina virtual é iniciada, você pode se conectar ao console da máquina virtual via VMConnect para instalar o sistema operacional. Selecione a máquina virtual na lista, clique em **mais** > **Connect** para baixar o arquivo. rdp. Abra o arquivo. RDP no aplicativo da Conexão de área de trabalho remota. Já que isso está se conectando ao console da máquina virtual, você precisará inserir credenciais de administrador do host do Hyper-V.

## Alterar configurações de máquina virtual

![Tela de configurações de máquina virtual](../media/manage-virtual-machines/vm-settings.png)

1. Clique na ferramenta de **máquinas virtuais** do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** escolha uma máquina virtual na lista e clique em **mais** > **configurações**.
3. Alternar entre a guia **Geral**, **segurança**, **memória**, **processadores**, **discos**, **redes**, **ordem de inicialização** e **pontos de verificação** , defina as configurações necessárias e clique em **Salvar** para salvar configurações da guia atual. As configurações disponíveis varia de acordo com a geração da máquina virtual. Além disso, algumas configurações não podem ser alteradas para máquinas virtuais em execução e você precisará parar a máquina virtual pela primeira vez.

## Migração ao vivo uma máquina virtual para outro nó de cluster

Se você estiver conectado a um cluster, você pode migrar dinâmicos uma máquina virtual para outro nó de cluster.

1. Em um Cluster de Failover ou conexão do cluster hiperconvergente, clique na ferramenta de **máquinas virtuais** do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** escolha uma máquina virtual na lista e clique em **mais** > **Mover**.
3. Escolha um servidor na lista de nós de cluster disponíveis e clique em **Mover**.
4. Notificações para o movimento de progresso serão exibidas no canto superior direito do Windows Admin Center. Se a movimentação for bem-sucedida, você verá o nome do servidor Host alterado na lista de máquina virtual.

## Gerenciamento avançado e solução de problemas para uma única máquina virtual

![Tela de detalhes da única máquina virtual](../media/manage-virtual-machines/vm-details.png)

Você pode exibir informações detalhadas e gráficos de desempenho para uma única máquina virtual de página única máquina virtual.

1. Clique na ferramenta de **máquinas virtuais** do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** clique no nome de uma máquina virtual na lista de máquina virtual.
3. Na página única máquina virtual, você pode:
    - Exiba informações detalhadas para a máquina virtual.
    - Exibir Live e gráficos de linhas de dados históricos de CPU, memória, rede, taxa de transferência IOPS e e/s (dados históricos só estão disponíveis para clusters hiperconvergentes executando o Windows Server 2019)
    - Exibir, criar, aplicar, renomear e excluir pontos de verificação.
    - Exibir detalhes de arquivos de disco rígido virtual (VHD) da máquina virtual, adaptadores de rede e servidor host.
    - Excluir, iniciar, desativar, desligar, pausar, retomar, redefinir ou renomear a máquina virtual. Também salvar a máquina virtual, excluir um estado salvo ou criar um ponto de verificação.
    - [Alterar configurações para a máquina virtual](#change-virtual-machine-settings).
    - Conecte o console da máquina virtual usando o VMConnect por meio do host do Hyper-V.
    - [Replicar a máquina virtual usando o Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## Gerenciar uma máquina virtual por meio do host do Hyper-V (VMConnect)

![VM se conectar por meio de seu navegador da web](../media/manage-virtual-machines/vm-connect.png)

1. Clique na ferramenta de **máquinas virtuais** do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **inventário** escolha uma máquina virtual na lista e clique em **mais** > **Connect** ou **arquivo RDP baixar**. **Conectar** permitirá que você interaja com o convidado VM por meio do console da web de área de trabalho remota, integrado ao Windows Admin Center. **Arquivo RDP baixar** baixará um arquivo. rdp que você pode abrir com o aplicativo de Conexão de área de trabalho remota (mstsc.exe). As duas opções usarão VMConnect para se conectar à VM convidado por meio do host do Hyper-V e exigem que você insira as credenciais de administrador para o servidor de host do Hyper-V.

## Alterar configurações de host do Hyper-V

![Tela de configurações do host Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. Em uma conexão de servidor, Cluster de hiperconvergência ou Cluster de Failover, clique no menu de **configurações** na parte inferior do painel de navegação do lado esquerdo.
2. Em um servidor de host do Hyper-V ou cluster, você verá um grupo de **Configurações de Host do Hyper-V** com as seções a seguir:
    - Geral: Alteração virtual discos rígidos e máquinas virtuais do caminho do arquivo e o hipervisor agendar tipo (se houver suporte)
    - Modo de sessão avançado
    - NUMA abrangência
    - Migração ao vivo
    - Migração de Armazenamento
3. Se você fizer qualquer host do Hyper-V alterações de configuração em um Cluster de Failover ou Cluster hiperconvergente conexão, a alteração será aplicada a todos os nós de cluster.

## Exibir logs de eventos do Hyper-V

Você pode exibir logs de eventos diretamente da ferramenta de máquinas virtuais do Hyper-V.

1. Clique na ferramenta de **máquinas virtuais** do painel de navegação do lado esquerdo.
2. Na parte superior da ferramenta máquinas virtuais, escolha a guia **Resumo** . Na seção superior direita eventos, clique em **Todos os eventos de exibição**.
3. A ferramenta Visualizador de eventos mostrará os canais de evento do Hyper-V no painel esquerdo. Escolha um canal para exibir os eventos no painel direito. Se você estiver gerenciando um cluster de failover ou cluster hiperconvergente, os logs de eventos exibirá eventos para todos os nós do cluster, exibindo o servidor host na coluna máquina.

## Proteger máquinas virtuais com o Azure Site Recovery

Você pode usar o Windows Admin Center para configurar o Azure Site Recovery e replicar suas máquinas virtuais de local para o Azure. [Saiba mais](../azure/azure-site-recovery.md)

## Aguarde mais

Gerenciamento de máquinas virtuais no Windows Admin Center está ativamente em desenvolvimento e novos recursos serão adicionados no futuro próximo. Você pode exibir o status e vote recursos em UserVoice:

|Solicitação de recurso|
|-------|
|[Importação/exportação máquinas virtuais](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[Máquinas virtuais de classificação em pastas](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[Configurações de máquinas virtuais adicionais de suporte](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[Suporte à réplica do Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[Delegar a propriedade de máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[Clone a máquina virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[Criar um modelo de uma máquina virtual existente](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[Máquinas virtuais de modo de exibição em todos os hosts Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[Configurar VLAN no painel de nova máquina Virtual](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**Ver todas as ou propor novo recurso**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
