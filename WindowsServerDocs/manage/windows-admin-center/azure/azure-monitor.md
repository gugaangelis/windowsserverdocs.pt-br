---
title: Monitorar os servidores e configurar alertas com o Azure Monitor do Windows Admin Center
description: Windows Admin Center (projeto Paulo) integra-se com o Azure Monitor
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 8f7baba465071cc95ab7492037ff25c5cd58219e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280432"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>Monitorar os servidores e configurar alertas com o Azure Monitor do Windows Admin Center

[Saiba mais sobre a integração do Azure com Windows Admin Center.](../plan/azure-integration-options.md)

[O Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) é uma solução que coleta, analisa e atua na telemetria de uma variedade de recursos, incluindo servidores Windows e VMs, tanto localmente quanto na nuvem. Embora o Azure Monitor extrai dados de VMs do Azure e outros recursos do Azure, este artigo se concentra em como o Azure Monitor funciona com servidores locais e VMs, especificamente com o Windows Admin Center. Se você estiver interessado em aprender como você pode usar o Azure Monitor para obter alertas por email sobre o cluster hiperconvergente, leia sobre [usando o Azure Monitor para enviar emails para falhas de serviço de integridade](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## <a name="how-does-azure-monitor-work"></a>Como funciona o Azure Monitor?
![img](../media/azure-monitor-diagram.png) gerados a partir de servidores do Windows no local de dados são coletados em um espaço de trabalho do Log Analytics no Azure Monitor. Dentro de um espaço de trabalho, você pode permitir que várias soluções de monitoramento — os conjuntos de lógica que fornecem informações para um cenário específico. Por exemplo, o gerenciamento de atualizações do Azure, a Central de segurança do Azure e Azure Monitor para as VMs são todas as soluções de monitoramento que podem ser habilitadas dentro de um espaço de trabalho. 

Quando você habilita uma solução de monitoramento em um espaço de trabalho do Log Analytics, todos os servidores subordinados a esse espaço de trabalho começará a coletar dados relevantes para essa solução, para que a solução possa gerar insights para todos os servidores no espaço de trabalho. 

Para coletar dados de telemetria em um servidor de local e enviá-los para o espaço de trabalho do Log Analytics, o Azure Monitor requer a instalação do Microsoft Monitoring Agent ou o MMA. Determinadas soluções de monitoramento também exigem um agente secundário. Por exemplo, Azure Monitor para VMs também depende de um agente do ServiceMap para funcionalidade adicional que fornece essa solução. 

Algumas soluções, como gerenciamento de atualizações do Azure, também dependerá da automação do Azure, que permite que você gerencie centralmente recursos nos ambientes de não Azure e do Azure. Por exemplo, o gerenciamento de atualizações do Azure usa a automação do Azure para agendar e orquestrar centralmente, instalação de atualizações em todos os computadores no seu ambiente, do portal do Azure.


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>Como Windows Admin Center permitem que você use o Azure Monitor?

WAC, você pode habilitar duas soluções de monitoramento:

- [Gerenciamento de atualizações do Azure](azure-update-management.md) (na ferramenta de atualizações)
- Monitor do Azure para VMs (em configurações do servidor), também conhecidas como máquinas virtuais insights

Você pode começar usando o Azure Monitor de qualquer uma dessas ferramentas. Se você nunca usou o Azure Monitor antes, WAC provisionará automaticamente um espaço de trabalho do Log Analytics (e conta de automação do Azure, se necessário) e instale e configure o agente MMA (Microsoft Monitoring) no servidor de destino. Em seguida, ele instalará a solução correspondente no espaço de trabalho. 

Por exemplo, se você primeiro vá para a ferramenta de atualizações para configurar o gerenciamento de atualização do Azure, será WAC:

1. Instale o MMA na máquina
2. Criar o espaço de trabalho do Log Analytics e a conta de automação do Azure (uma vez que uma conta de automação do Azure é necessária neste caso)
3. Instale a solução de gerenciamento de atualizações no espaço de trabalho recém-criada.

Se você quiser adicionar outra solução de monitoramento de dentro do WAC no mesmo servidor, WAC simplesmente instalará essa solução no espaço de trabalho existente ao qual esse servidor está conectado. Além disso, WAC instalará todos os outros agentes necessários.

Se você se conectar a um servidor diferente, mas já configurou um espaço de trabalho do Log Analytics (seja por meio de WAC ou manualmente no Portal do Azure), também pode instalar o agente MMA no servidor e conectá-lo até um espaço de trabalho. Quando você se conecta a um servidor em um espaço de trabalho, ele começará automaticamente a coleta de dados e relatórios para soluções instaladas no espaço de trabalho.

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Monitorar o Azure para máquinas virtuais (também conhecido como Insights de máquina virtual)
>Aplica-se a: Windows Admin Center Preview

Quando você define o Monitor do Azure para VMs no servidor de configurações, Windows Admin Center permite que o Azure Monitor para solução de VMs, também conhecido como insights de máquina Virtual. Essa solução permite que você monitore eventos e a integridade do servidor, criar alertas de email, obter uma exibição consolidada do desempenho do servidor em seu ambiente e visualizar os aplicativos, sistemas e serviços conectados a um determinado servidor.

> [!NOTE]
> Apesar do nome, insights VM funciona para servidores físicos, bem como as máquinas virtuais.

Com o Azure Monitor livre 5 GB de bonificação de dados/mês/cliente, você pode facilmente experimentar isso para um servidor ou dois sem se preocupar de serem cobrados. Continue lendo para ver os benefícios adicionais de servidores de integração para o Azure Monitor, como obter uma exibição consolidada do desempenho de sistemas entre os servidores no seu ambiente.

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**Configurar seu servidor para uso com o Azure Monitor**

Na página Visão geral de uma conexão de servidor, clique no novo botão "Gerenciar alertas" ou vá para configurações do servidor > monitoramento e alertas. Nessa página, integrar seu servidor para o Azure Monitor, clicando em "configurar" e Concluindo o painel de configuração. Centro de administração se encarrega de provisionamento de espaço de trabalho do Log Analytics do Azure, instalar o agente necessário e garantir que a solução de insights VM está configurada. Uma vez concluído, o servidor enviará dados de contador de desempenho para o Azure Monitor, permitindo que você exibir e criar alertas de email com base nesse servidor, do portal do Azure.

### <a name="create-email-alerts"></a>**Criar alertas de email**

Depois que você já anexou o seu servidor para o Azure Monitor, você pode usar os hiperlinks inteligentes dentro das configurações > página de monitoramento e alertas para navegar até o Portal do Azure. Centro de administração habilita automaticamente os contadores de desempenho a serem coletados, para que você possa facilmente [criar um novo alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) Personalizando uma das muitas consultas predefinidas ou escrever seu próprio.

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>\* * Obter uma exibição consolidada em vários servidores * *

Se você integrar vários servidores para um único espaço de trabalho do Log Analytics no Azure Monitor, você pode obter uma exibição consolidada de todos esses servidores do [solução de máquinas virtuais Insights](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) dentro do Azure Monitor.  (Observe que somente as guias de desempenho e mapas de Insights de máquinas virtuais para o Azure Monitor funcione com servidores locais – as funções da guia de integridade apenas com VMs do Azure). Para exibir isso no portal do Azure, vá para o Azure Monitor > máquinas virtuais (no Insights) e navegue até as guias de "Desempenho" ou "Mapas".

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**Visualizar aplicativos, sistemas e serviços conectados a um determinado servidor**

Quando integra um servidor para a solução de insights VM no Azure Monitor do Centro de administração, ele também acende uma funcionalidade chamada [mapa do serviço](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Esse recurso automaticamente descobre os componentes do aplicativo e mapeia a comunicação entre serviços, para que você possa visualizar facilmente as conexões entre servidores com detalhes do portal do Azure. Você pode encontrá-lo indo até o portal do Azure > Azure Monitor > máquinas virtuais (no Insights) e navegando até a guia "Mapas".

> [!NOTE]
> As visualizações para Insights de máquinas virtuais do Azure Monitor são oferecidas em regiões públicas 6 no momento.  Para obter as informações mais recentes, verifique as [do Azure Monitor para obter a documentação de VMs](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Você deve implantar o espaço de trabalho do Log Analytics em uma das regiões com suporte para obter os benefícios adicionais fornecidos pela solução de máquinas virtuais Insights descrita acima.

## <a name="disabling-monitoring"></a>Desabilitando o monitoramento

Para se desconectar completamente seu servidor do espaço de trabalho do Log Analytics, desinstale o agente MMA. Isso significa que este servidor não enviará dados para o espaço de trabalho, e todas as soluções instaladas no espaço de trabalho não poderão mais coletam e processam dados desse servidor. No entanto, isso não afeta o espaço de trabalho em si – todos os recursos de emissão de relatórios para esse espaço de trabalho continuará a fazê-lo. Para desinstalar o agente MMA em WAC, vá para aplicativos e recursos, encontre o Microsoft Monitoring Agent e clique em Desinstalar.

Se você quiser desativar uma solução específica dentro de um espaço de trabalho, você precisará [remover a solução de monitoramento do portal do Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). Remover uma solução de monitoramento significa que os insights criados por essa solução não serão gerados para _qualquer_ dos servidores subordinados a esse espaço de trabalho. Por exemplo, se desinstalar o Azure Monitor para a solução de VMs, eu não verá mais insights sobre o desempenho de VM ou servidor de qualquer um dos computadores conectados ao meu espaço de trabalho.