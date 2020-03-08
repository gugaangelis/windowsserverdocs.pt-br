---
title: Monitorar servidores e configurar alertas com o Azure Monitor do centro de administração do Windows
description: O centro de administração do Windows (Project Honolulu) integra-se com o Azure Monitor
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 03/24/2019
ms.openlocfilehash: 28108a79bbdc654f6437a698c158a3f74d4423ba
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371124"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>Monitorar servidores e configurar alertas com o Azure Monitor do centro de administração do Windows

[Saiba mais sobre a integração do Azure com o centro de administração do Windows.](../plan/azure-integration-options.md)

[Azure monitor](https://docs.microsoft.com/azure/azure-monitor/overview) é uma solução que coleta, analisa e atua na telemetria de uma variedade de recursos, incluindo servidores Windows e VMS, tanto localmente quanto na nuvem. Embora Azure Monitor receba dados de VMs do Azure e outros recursos do Azure, este artigo se concentra em como o Azure Monitor funciona com servidores e VMs locais, especificamente com o centro de administração do Windows. Se você estiver interessado em saber como usar Azure Monitor para receber alertas de email sobre o cluster hiperconvergente, leia sobre como [usar Azure monitor para enviar emails para serviço de integridade falhas](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## <a name="how-does-azure-monitor-work"></a>Como funciona Azure Monitor?
![img](../media/azure-monitor-diagram.png) dados gerados de servidores locais do Windows são coletados em um espaço de trabalho Log Analytics no Azure Monitor. Em um espaço de trabalho, você pode habilitar várias soluções de monitoramento — conjuntos de lógica que fornecem informações para um cenário específico. Por exemplo, Gerenciamento de Atualizações do Azure, a central de segurança do Azure e Azure Monitor para VMs são soluções de monitoramento que podem ser habilitadas em um espaço de trabalho. 

Quando você habilita uma solução de monitoramento em um espaço de trabalho Log Analytics, todos os servidores que se reportam a esse espaço de trabalho começarão a coletar dados relevantes para essa solução, para que a solução possa gerar insights para todos os servidores no espaço de trabalho. 

Para coletar dados de telemetria em um servidor local e enviá-los para o espaço de trabalho Log Analytics, Azure Monitor requer a instalação do Microsoft Monitoring Agent ou o MMA. Determinadas soluções de monitoramento também exigem um agente secundário. Por exemplo, Azure Monitor para VMs também depende de um agente ServiceMap para funcionalidade adicional que essa solução fornece. 

Algumas soluções, como o Azure Gerenciamento de Atualizações, também dependem da automação do Azure, que permite que você gerencie centralmente recursos em ambientes do Azure e não Azure. Por exemplo, o Azure Gerenciamento de Atualizações usa a automação do Azure para agendar e orquestrar a instalação de atualizações entre computadores em seu ambiente, de forma centralizada, do portal do Azure.


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>Como o centro de administração do Windows permite que você use Azure Monitor?

De dentro do WAC, você pode habilitar duas soluções de monitoramento:

- [Gerenciamento de atualizações do Azure](azure-update-management.md) (na ferramenta de atualizações)
- Azure Monitor para VMs (em configurações do servidor), a. k. um insights de máquinas virtuais

Você pode começar a usar o Azure Monitor de qualquer uma dessas ferramentas. Se você nunca usou Azure Monitor antes, o WAC provisionará automaticamente um espaço de trabalho Log Analytics (e uma conta de automação do Azure, se necessário), e instalará e configurará o Microsoft Monitoring Agent (MMA) no servidor de destino. Em seguida, ele instalará a solução correspondente no espaço de trabalho. 

Por exemplo, se você primeiro acessar a ferramenta atualizações para configurar o Azure Gerenciamento de Atualizações, o WAC irá:

1. Instalar o MMA no computador
2. Criar o espaço de trabalho Log Analytics e a conta de automação do Azure (já que uma conta de automação do Azure é necessária neste caso)
3. Instale a solução Gerenciamento de Atualizações no espaço de trabalho recém-criado.

Se você quiser adicionar outra solução de monitoramento de dentro do WAC no mesmo servidor, o WAC simplesmente instalará essa solução no espaço de trabalho existente ao qual o servidor está conectado. O WAC também instalará outros agentes necessários.

Se você se conectar a um servidor diferente, mas já tiver configurado um Log Analytics espaço de trabalho (por meio de WAC ou manualmente no portal do Azure), você também poderá instalar o agente do MMA no servidor e conectá-lo a um espaço de trabalho existente. Quando você conecta um servidor a um espaço de trabalho, ele inicia automaticamente a coleta de dados e a geração de relatórios para soluções instaladas nesse espaço de trabalho.

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Azure Monitor para máquinas virtuais (também conhecido como Insights de máquina virtual)
>Aplica-se a: versão prévia do centro de administração do Windows

Quando você define Azure Monitor para VMs nas configurações do servidor, o centro de administração do Windows habilita a solução Azure Monitor para VMs, também conhecida como insights de máquina virtual. Essa solução permite monitorar a integridade e os eventos do servidor, criar alertas de email, obter uma exibição consolidada do desempenho do servidor em seu ambiente e Visualizar aplicativos, sistemas e serviços conectados a um determinado servidor.

> [!NOTE]
> Apesar de seu nome, o VM insights funciona para servidores físicos, bem como para máquinas virtuais.

Com 5 GB gratuitos de dados/mês/concessão de cliente do Azure Monitor, você pode facilmente experimentar para um servidor ou dois sem se preocupar em ser cobrado. Continue lendo para ver os benefícios adicionais da integração de servidores no Azure Monitor, como obter uma visão consolidada do desempenho dos sistemas em todos os servidores em seu ambiente.

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**Configurar seu servidor para uso com o Azure Monitor**

Na página Visão geral de uma conexão de servidor, clique no botão novo "Gerenciar alertas" ou vá para configurações do servidor > monitoramento e alertas. Nessa página, integre seu servidor ao Azure Monitor clicando em "configurar" e concluindo o painel de instalação. O centro de administração cuida do provisionamento do espaço de trabalho Log Analytics do Azure, instalando o agente necessário e garantindo que a solução de informações da VM esteja configurada. Depois de concluído, o servidor enviará dados do contador de desempenho para Azure Monitor, permitindo que você exiba e crie alertas de email com base nesse servidor, no portal do Azure.

### <a name="create-email-alerts"></a>**Criar alertas de email**

Depois de conectar o servidor ao Azure Monitor, você poderá usar os hiperlinks inteligentes dentro da página Configurações > monitoramento e alertas para navegar até o portal do Azure. O centro de administração permite que os contadores de desempenho sejam coletados automaticamente, para que você possa [criar facilmente um novo alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) Personalizando uma das muitas consultas predefinidas ou escrevendo as suas próprias.

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>\* * Obter uma exibição consolidada entre vários servidores * *

Se você integrar vários servidores a um único espaço de trabalho de Log Analytics no Azure Monitor, poderá obter uma exibição consolidada de todos esses servidores da [solução de informações de máquinas virtuais](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) no Azure monitor.  (Observe que apenas as guias desempenho e mapas das informações de máquinas virtuais para Azure Monitor funcionarão com servidores locais – a guia de integridade funciona somente com as VMs do Azure.) Para exibir isso na portal do Azure, acesse Azure Monitor > máquinas virtuais (em insights) e navegue até as guias "desempenho" ou "mapas".

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**Visualizar aplicativos, sistemas e serviços conectados a um determinado servidor**

Quando o centro de administração integra um servidor na solução de informações da VM dentro do Azure Monitor, ele também acende um recurso chamado [mapa do serviço](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Esse recurso descobre automaticamente os componentes do aplicativo e mapeia a comunicação entre os serviços para que você possa visualizar facilmente as conexões entre os servidores com muitos detalhes do portal do Azure. Você pode encontrá-lo acessando o portal do Azure > Azure Monitor > máquinas virtuais (em insights) e navegando até a guia "mapas".

> [!NOTE]
> No momento, as visualizações para as informações de máquinas virtuais para Azure Monitor são oferecidas em 6 regiões públicas.  Para obter as informações mais recentes, consulte a [documentação do Azure monitor para VMs](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Você deve implantar o espaço de trabalho Log Analytics em uma das regiões com suporte para obter os benefícios adicionais fornecidos pela solução de informações de máquinas virtuais descrita acima.

## <a name="disabling-monitoring"></a>Desabilitando o monitoramento

Para desconectar completamente o servidor do espaço de trabalho Log Analytics, desinstale o agente MMA. Isso significa que esse servidor não enviará mais dados para o espaço de trabalho, e todas as soluções instaladas nesse espaço de trabalho não coletarão e processarão dados desse servidor. No entanto, isso não afeta o próprio espaço de trabalho – todos os recursos que se reportam a esse espaço de trabalho continuarão a fazer isso. Para desinstalar o agente do MMA em WAC, vá para aplicativos & recursos, localize o Microsoft Monitoring Agent e clique em desinstalar.

Se você quiser desativar uma solução específica em um espaço de trabalho, será necessário [remover a solução de monitoramento do portal do Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). A remoção de uma solução de monitoramento significa que as informações criadas pela solução não serão mais geradas para _qualquer_ um dos servidores que se reportam a esse espaço de trabalho. Por exemplo, se eu desinstalar a solução Azure Monitor para VMs, não verá mais informações sobre o desempenho da VM ou do servidor de qualquer uma das máquinas conectadas ao meu espaço de trabalho.