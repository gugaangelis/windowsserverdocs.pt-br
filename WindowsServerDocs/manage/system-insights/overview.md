---
title: Visão geral dos insights do sistema
description: O System insights é um novo recurso de análise preditiva no Windows Server 2019. Os recursos de previsão do System insights – cada um apoiado por um modelo de aprendizado de máquina – analise localmente os dados do sistema do Windows Server, como contadores de desempenho e eventos, fornecendo informações sobre o funcionamento dos servidores e ajudando a reduzir as despesas operacionais associadas ao gerenciamento reativo de problemas em suas implantações.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: b1f0fc5343c5228a02369a64bff2de50ab3f863e
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471761"
---
# <a name="system-insights-overview"></a>Visão geral dos insights do sistema

>Aplica-se a: Windows Server 2019

O System insights é um novo recurso de análise preditiva no Windows Server 2019. Os recursos de previsão do System insights – cada um apoiado por um modelo de aprendizado de máquina – analise localmente os dados do sistema do Windows Server, como contadores de desempenho e eventos, fornecendo informações sobre o funcionamento dos servidores e ajudando a reduzir as despesas operacionais associadas ao gerenciamento reativo de problemas em suas implantações.

No Windows Server 2019, o System insights é fornecido com quatro recursos padrão com foco na previsão de capacidade, prevendo recursos futuros para computação, rede e armazenamento com base em seus padrões de uso anteriores. O System insights também é fornecido com uma [infraestrutura extensível](adding-and-developing-capabilities.md), portanto, a Microsoft e terceiros podem adicionar novos recursos de previsão ao insights do sistema sem Atualizar o sistema operacional.

Você pode gerenciar as informações do sistema por meio de uma extensão intuitiva do [centro de administração do Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) ou [diretamente por meio do PowerShell](https://aka.ms/SystemInsightsPowerShell), e o System insights permite que você configure cada recurso de previsão separadamente de acordo com as necessidades de sua implantação. Todos os resultados de previsão são publicados no log de eventos, o que permite que você use [Azure monitor](https://azure.microsoft.com/services/monitor/) ou [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) para agregar facilmente e ver previsões em um grupo de computadores.

![Extensão do System insights no centro de administração do Windows, mostrando a capacidade de previsão de capacidade da CPU com um gráfico que plota a previsão](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>Funcionalidade local
O System insights é executado completamente localmente no Windows Server. Usando a nova funcionalidade introduzida no Windows Server 2019, todos os seus dados são coletados, mantidos e analisados diretamente em seu computador, permitindo que você perceba os recursos de análise preditiva sem nenhuma conectividade de nuvem.

Os dados do sistema são armazenados em seu computador, e esses dados são analisados por recursos de previsão que não exigem o novo treinamento na nuvem. Com o System insights, você pode manter seus dados em seu computador e ainda se beneficiar dos recursos de análise preditiva.

## <a name="get-started"></a>Introdução

<iframe src=https://www.youtube-nocookie.com/embed/AJxQkx5WSaA width=560 height=315 allowfullscreen></iframe>

>[!TIP]
>Assista a esses vídeos curtos para aprender as informações de que você precisa para começar e gerenciar com segurança o System insights: [introdução ao System insights em 10 minutos](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>Requisitos
As informações do sistema estão disponíveis em qualquer instância do Windows Server 2019. Ele é executado em máquinas de host e convidado, em qualquer hipervisor e em qualquer nuvem.

### <a name="install-system-insights"></a>Instalar o System insights
>[!IMPORTANT]
>As informações do sistema coletam e armazenam até um ano de dados no nível local. Se você quiser manter seus dados ao atualizar seu sistema operacional, **certifique-** se de usar a atualização in-loco.

#### <a name="install-the-feature"></a>Instalar o recurso
Você pode instalar o System insights usando a extensão no centro de administração do Windows:

![Experiência do dia 0 para a extensão do System insights.](media/day-0-2.png)

Você também pode instalar o System insights diretamente por meio de Gerenciador do Servidor adicionando o recurso **System-insights** ou usando o PowerShell:

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>Fornecer comentários
Adoraríamos saber seus comentários para nos ajudar a melhorar esse recurso. Você pode usar os seguintes canais para enviar comentários:
- **Hub de comentários**: Use a ferramenta de Hub de comentários no Windows 10 para arquivar um bug ou comentários. Ao fazer isso, especifique:
    - **Categoria**: servidor
    - **Subcategoria**: informações do sistema
- **UserVoice**: envie solicitações de recursos por meio de nossa [página uservoice](https://windowsserver.uservoice.com/forums/295071-management-tools). Compartilhe com seus colegas para votar em itens que são importantes para você.
- **Email**: se você quiser enviar seus comentários privadamente para a equipe de recursos, envie um email para system-insights-feed@microsoft.com . Tenha em mente que ainda poderemos solicitar que você use o Hub de comentários ou UserVoice.

## <a name="additional-references"></a>Referências adicionais
Para saber mais sobre o System insights, use os seguintes recursos:

- [Noções básicas dos recursos](understanding-capabilities.md)
- [Gerenciar recursos](managing-capabilities.md)
- [Adicionar e desenvolver recursos](adding-and-developing-capabilities.md)
- [Perguntas frequentes do System insights](faq.md)