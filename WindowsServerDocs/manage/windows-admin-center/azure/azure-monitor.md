---
title: Monitorar servidores e configurar alertas com o Azure Monitor do Windows Admin Center
description: Windows Admin Center (Project Honolulu) se integra ao Monitor do Azure
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 6ada708bf7dd8cd08e1bc2620be5244a07beac7d
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296851"
---
# Monitorar servidores e configurar alertas com o Azure Monitor do Windows Admin Center

[Saiba mais sobre a integração do Azure com o Windows Admin Center.](../plan/azure-integration-options.md)

[Monitor do Azure](https://docs.microsoft.com/azure/azure-monitor/overview) é uma solução que coleta, analisa e atua sobre telemetria dentre uma variedade de recursos, incluindo servidores do Windows e VMs, no local e na nuvem. Embora o Azure Monitor recebe dados de VMs do Azure e outros recursos do Azure, este artigo se concentra em como o Monitor do Azure funciona com servidores locais e VMs, especificamente com o Windows Admin Center. Se você estiver interessado saber como você pode usar o Monitor do Azure para obter alertas de email sobre seu cluster hiperconvergente, leia sobre como [usar o Monitor do Azure para enviar emails para falhas de serviço de integridade](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## Como funciona o Monitor do Azure?
![img](../media/azure-monitor-diagram.png) gerados a partir de servidores do Windows no local de dados são coletados em um espaço de trabalho no Monitor do Azure Log Analytics. Dentro de um espaço de trabalho, você pode habilitar várias soluções de monitoramento — conjuntos de lógica que fornecer dados para um cenário específico. Por exemplo, gerenciamento de atualização do Azure, Central de segurança do Azure e Monitor do Azure para VMs são todas as soluções de monitoramento que podem ser habilitadas dentro de um espaço de trabalho. 

Quando você habilita uma solução de monitoramento em um espaço de trabalho do Log Analytics, todos os servidores de relatório para esse espaço de trabalho Iniciar a coleta de dados relevantes para que essa solução, para que a solução pode gerar insights para todos os servidores no espaço de trabalho. 

Para coletar dados de telemetria em um servidor local e enviar por push para o espaço de trabalho do Log Analytics, Monitor Azure requer a instalação do Microsoft Monitoring Agent ou o MMA. Determinadas soluções de monitoramento também exigem um agente secundário. Por exemplo, Monitor do Azure para VMs também depende de um agente ServiceMap para funcionalidade adicional que fornece essa solução. 

Algumas soluções, como o gerenciamento de atualização do Azure, também dependem de automação do Azure, que permite que você gerencie centralmente recursos em ambientes de não Azure e o Azure. Por exemplo, o gerenciamento de atualização do Azure usa Azure automação para agendar e orquestrar centralmente, instalação de atualizações em todos os computadores em seu ambiente, do portal do Azure.


## Como Windows Admin Center permitem que você use o Monitor do Azure?

WAC, você pode habilitar duas soluções de monitoramento:

- [Gerenciamento de atualização do Azure](azure-update-management.md) (na ferramenta atualizações)
- Azure Monitor para VMs (em configurações do servidor), também conhecido como máquinas virtuais insights

Você pode começar a usar Azure Monitor de qualquer uma dessas ferramentas. Se você nunca tiver usado o Monitor Azure antes, WAC será provisionada automaticamente um espaço de trabalho do Log Analytics (e conta de automação do Azure, se necessário) e instalar e configurar o Microsoft Monitoring Agent (MMA) no servidor de destino. Em seguida, ele instalará a solução correspondente na área de trabalho. 

Por exemplo, se você primeiro vá para a ferramenta de atualizações para configurar o gerenciamento de atualização do Azure, WAC será:

1. Instalar o MMA no computador
2. Criar o espaço de trabalho do Log Analytics e a conta do Azure automação (como uma conta do Azure automação é necessária neste caso)
3. Instale a solução de gerenciamento de atualizações no espaço de trabalho recém-criado.

Se você quiser adicionar outra solução de monitoramento de dentro do WAC no mesmo servidor, WAC simplesmente instalar essa solução na área de trabalho existente ao qual o servidor está conectado. Além disso, WAC instalará quaisquer outros agentes necessários.

Se você se conectar a um servidor diferente, mas já tenha configurado um espaço de trabalho do Log Analytics (seja por meio de WAC ou manualmente no Portal do Azure), você também pode instalar o agente MMA no servidor e conectá-lo até um espaço de trabalho existente. Quando você se conecta a um servidor em um espaço de trabalho, ele inicia automaticamente coleta de dados e relatórios para soluções instaladas no espaço de trabalho.

## Monitorar o Azure para máquinas virtuais (ou seja Percepções da máquina virtual)
>Aplicável a: Windows Admin Center Preview

Quando você define o Monitor do Azure para VMs em configurações do servidor, o Windows Admin Center permite que o Monitor do Azure para solução de VMs, também conhecido como insights de máquina Virtual. Essa solução permite que você monitorar a integridade do servidor e eventos, criar alertas de email, obtenha uma visão consolidada de desempenho do servidor em seu ambiente e visualizar serviços conectados a um determinado servidor, sistemas e aplicativos.

> [!NOTE]
> Apesar do nome, insights VM funciona para servidores físicos, bem como máquinas virtuais.

Com o Azure do Monitor livre 5 GB de dados/mês/cliente extra, você pode facilmente experimentar isso para um servidor ou dois sem se preocupar obtendo cobrado. Leia para ver os benefícios adicionais de servidores de integração no Monitor do Azure, como obter uma visão consolidada de desempenho de sistemas em todos os servidores no seu ambiente.

### **Configurar seu servidor para uso com o Monitor do Azure**

Na página de visão geral de uma conexão de servidor, clique no botão Novo "Gerenciar alertas" ou, vá para configurações do servidor gt _ monitoramento e alertas. Nessa página, integrar seu servidor para Azure Monitor, clicando em "configurar" e concluir o painel de configuração. Admin Center se encarrega de provisionamento do espaço de trabalho do Azure Log Analytics, instalando o agente necessário e garantir que a solução de insights VM está configurado. Depois de concluído, o servidor de enviar dados do contador de desempenho para Monitor do Azure, permitindo que você exiba e crie alertas de email com base nesse servidor, do portal do Azure.

### **Criar alertas de email**

Depois que você afixadas seu servidor ao Monitor do Azure, você pode usar os hiperlinks inteligentes dentro da página de configurações gt _ monitoramento e alertas para navegar para o Portal do Azure. Centro de administração habilita automaticamente os contadores de desempenho para ser coletados, portanto, você pode facilmente [criar um novo alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) por personalizar um dos muitos consultas predefinidas ou estiver criando seu próprio.

### * * Obter uma visão consolidada em vários servidores * *

Se você integrar vários servidores para um único espaço de trabalho do Log Analytics dentro do Monitor do Azure, você pode obter uma visão consolidada de todos esses servidores da [solução de máquinas virtuais Insights](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) dentro do Monitor do Azure.  (Observe que somente as guias de desempenho e mapas de informações de máquinas virtuais para Azure Monitor funcionará com servidores locais – as funções de guia integridade somente com VMs do Azure). Para ver isso no portal do Azure, vá para Azure Monitor gt _ máquinas virtuais (em Insights) e navegue até as guias "Desempenho" ou "Maps".

### **Visualizar serviços conectados a um determinado servidor, sistemas e aplicativos**

Quando Admin Center onboards um servidor na solução de percepções da VM dentro do Monitor do Azure, ele também acende uma funcionalidade chamada de [Serviço](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Essa funcionalidade automaticamente detecta componentes de aplicativo e mapeia a comunicação entre serviços para que você pode facilmente visualizar conexões entre servidores com detalhes do portal do Azure. Você pode descobrir isso indo para o Azure portal gt _ gt _ Azure Monitor máquinas virtuais (em Insights), e navegar até a guia "Maps".

> [!NOTE]
> As visualizações para máquinas virtuais Insights para Azure Monitor são oferecidas em 6 regiões públicas no momento.  As informações mais recentes, verifique se o [Monitor do Azure para obter a documentação VMs](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Você deve implantar o espaço de trabalho do Log Analytics em uma das regiões com suporte para obter os benefícios adicionais fornecidos pela solução de máquinas virtuais Insights descrita acima.

## Desativando o monitoramento

Para desconectar completamente seu servidor do espaço de trabalho do Log Analytics, desinstale o agente MMA. Isso significa que esse servidor não enviará mais dados ao espaço de trabalho, e todas as soluções instaladas no espaço de trabalho não poderá mais coletam e processam dados do servidor. No entanto, isso não afeta o espaço de trabalho em si – todos os recursos de relatório para esse espaço de trabalho continuará a fazer isso. Para desinstalar o agente MMA dentro WAC, vá para aplicativos & recursos, localizar o Microsoft Monitoring Agent e clique em Desinstalar.

Se você deseja desativar uma solução específica dentro de um espaço de trabalho, você precisará [Remover a solução de monitoramento do portal do Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). Remover uma solução de monitoramento significa que as percepções criado por que solução não será gerada para _todos_ os servidores de relatório para esse espaço de trabalho. Por exemplo, se desinstalar o o Monitor do Azure para soluções de VMs, eu não verá mais ideias sobre o desempenho de VM ou servidor de qualquer um dos computadores conectados ao meu espaço de trabalho.