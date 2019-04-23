---
title: Visão geral de informações do sistema
description: Insights de sistema é um novo recurso de análise preditiva no Windows Server 2019. Os Insights de sistema recursos de previsão - cada sustentados por um modelo de aprendizado de máquina - localmente analisar dados de sistema do Windows Server, como contadores de desempenho e eventos, fornecendo informações sobre o funcionamento dos seus servidores e ajuda a reduzir a despesas operacionais associadas ao gerenciamento de forma reativa problemas em suas implantações.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: ca471e5e747c7aaa369f5725290e16deab7a6660
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887237"
---
# <a name="system-insights-overview"></a>Visão geral de informações do sistema

>Aplica-se a: Windows Server 2019

Insights de sistema é um novo recurso de análise preditiva no Windows Server 2019. Os Insights de sistema recursos de previsão - cada sustentados por um modelo de aprendizado de máquina - localmente analisar dados de sistema do Windows Server, como contadores de desempenho e eventos, fornecendo informações sobre o funcionamento dos seus servidores e ajuda a reduzir a despesas operacionais associadas ao gerenciamento de forma reativa problemas em suas implantações. 

No Windows Server de 2019, sistema Insights é fornecido com quatro recursos padrão com foco na capacidade de previsão, prevendo futuras de recursos de computação, rede e armazenamento com base em seus padrões de uso anterior. Informações do sistema também é fornecido com uma [infraestrutura extensível](adding-and-developing-capabilities.md), portanto, a Microsoft e 3ª partes podem adicionar novos recursos de previsão para o sistema Insights sem atualizar o sistema operacional. 

Você pode gerenciar o sistema Insights por meio de um intuitiva [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) extensão ou [diretamente por meio do PowerShell](https://aka.ms/SystemInsightsPowerShell), e o sistema Insights permite que você configure cada funcionalidade preditiva separadamente, de acordo com as necessidades da sua implantação. Todos os resultados de previsão são publicados no log de eventos, que permite que você use [do Azure Monitor](https://azure.microsoft.com/services/monitor/) ou [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) agregar e consulte previsões em um grupo de computadores com facilidade.

![Extensão de Insights de sistema no Windows Admin Center, mostrando a capacidade de previsão com um gráfico plotando a previsão de capacidade de CPU](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>Funcionalidade de local
Informações do sistema é executado completamente localmente no Windows Server. Usando a nova funcionalidade introduzida no Windows Server 2019, todos os seus dados é coletado, persistente e analisados diretamente em seu computador, permitindo que você aproveite os recursos de análise preditiva, sem qualquer conectividade de nuvem.

Seus dados de sistema são armazenados em seu computador, e esses dados são analisados pelos recursos de previsão que não exigem a readaptação na nuvem. Com o sistema Insights, você pode reter seus dados em seu computador e ainda aproveitar os recursos de análise preditiva. 

## <a name="get-started"></a>Introdução

<iframe src="https://www.youtube-nocookie.com/embed/AJxQkx5WSaA" width="560" height="315" allowfullscreen></iframe>

>[!TIP]
>Assista a esses vídeos curtos para saber as informações necessárias para começar a usar e gerenciar com confiança informações do sistema: [Introdução ao sistema Insights em 10 minutos](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>Requisitos
Informações do sistema está disponível em qualquer instância do Windows Server 2019. Ele é executado em máquinas de host e convidado, em qualquer hipervisor e em qualquer nuvem.

### <a name="install-system-insights"></a>Instale o sistema Insights
>[!IMPORTANT]
>Coleta de informações do sistema e armazena até um ano de dados localmente. Se você quiser reter seus dados ao atualizar seu sistema operacional, **Certifique-se de usar a atualização in-loco**.

#### <a name="install-the-feature"></a>Instalar o recurso
Você pode instalar os Insights de sistema usando a extensão de dentro do Windows Admin Center:

![Experiência de dia 0 para a extensão de informações do sistema.](media/day-0-2.png)

Você também pode instalar o sistema Insights diretamente por meio do Gerenciador do servidor, adicionando a **Insights de sistema** recurso, ou por meio do PowerShell:

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>Fornecer comentários
Adoraríamos ouvir seus comentários para nos ajudar a melhorar este recurso. Você pode usar os seguintes canais para enviar comentários:
- **Hub de comentários**: Use a ferramenta de Hub de comentários no Windows 10 em um arquivo de um bug ou comentários. Ao fazer isso, especifique:
    - **Categoria**: Servidor 
    - **Subcategoria**: Insights do Sistema
- **UserVoice**: Enviar solicitações de recursos por meio de nosso [página de UserVoice](https://windowsserver.uservoice.com/forums/295071-management-tools). Compartilhar com seus colegas para votar a favor dos itens que são importantes para você.
- **Email**: Se você quiser enviar seus comentários em particular para a equipe de recursos, envie um email para system-insights-feed@microsoft.com. Tenha em mente que ainda pode solicitamos que você usar o Hub de comentários do UserVoice.

## <a name="see-also"></a>Consulte também
Para saber mais sobre o sistema Insights, use os seguintes recursos:

- [Recursos de compreensão](understanding-capabilities.md)
- [Gerenciamento de recursos](managing-capabilities.md)
- [Adicionando e desenvolvimento de recursos](adding-and-developing-capabilities.md)
- [Perguntas Frequentes de Insights de sistema](faq.md)