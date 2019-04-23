---
title: Gerenciamento de recursos
description: Insights de sistema expõe uma variedade de configurações que podem ser configuradas para cada recurso, e essas configurações podem ser ajustadas para um endereço específico precisa de sua implantação. Este tópico descreve como gerenciar as várias configurações para cada capacidade por meio do Windows Admin Center ou PowerShell, fornecendo exemplos básicos do PowerShell e Windows Admin Center capturas de tela para demonstrar como ajustar essas configurações.
ms.custom: na
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 9081a0b576ab9871b47df38255047b6cbe889419
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868017"
---
# <a name="managing-capabilities"></a>Gerenciamento de recursos

>Aplica-se a: Windows Server 2019

No Windows Server de 2019, Insights de sistema expõe uma variedade de configurações que podem ser configuradas para cada recurso e essas configurações podem ser ajustadas para um endereço específico precisa de sua implantação. Este tópico descreve como gerenciar as várias configurações para cada capacidade por meio do Windows Admin Center ou PowerShell, fornecendo exemplos básicos do PowerShell e Windows Admin Center capturas de tela para demonstrar como ajustar essas configurações. 

>[!TIP]
>Você também pode usar esses vídeos curtos para ajudá-lo a começar a usar e gerenciar com confiança informações do sistema: [Introdução ao sistema Insights em 10 minutos](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

Embora esta seção fornece exemplos do PowerShell, você pode usar o [documentação do sistema Insights PowerShell](https://aka.ms/systeminsightspowershell) para ver todos os conjuntos de cmdlets, parâmetros e parâmetro dentro do sistema de informações. 

## <a name="viewing-capabilities"></a>Recursos de visualização

Para começar, você pode listar todos os recursos disponíveis usando o **Get-InsightsCapability** cmdlet: 

```PowerShell
Get-InsightsCapability
``` 
Esses recursos também são visíveis na extensão de informações do sistema:

![Página de visão geral de informações de sistema listando os recursos disponíveis](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>Habilitando e desabilitando um recurso
Cada recurso pode ser habilitado ou desabilitado. Desabilitar um recurso impede que esse recurso que está sendo invocado e recursos não padrão, desabilitar um recurso de interrompe toda coleta de dados para esse recurso. Por padrão, todos os recursos estão habilitados, e você pode verificar o estado de um recurso usando o **Get-InsightsCapability** cmdlet. 

Para habilitar ou desabilitar um recurso, use o **Enable-InsightsCapability** e o **Disable InsightsCapability** cmdlets:

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
``` 
Essas configurações também podem ser alternadas pelo selecionado uma funcionalidade no Windows Admin Center clicando o **habilitar** ou **desabilitar** botões.

### <a name="invoking-a-capability"></a>Invocar um recurso
Invocar uma funcionalidade imediatamente executa o recurso de recuperação de uma previsão e os administradores podem invocar qualquer tempo de um recurso clicando o **Invoke** botão no Windows Admin Center ou usando o  **InsightsCapability invocar** cmdlet:

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>Para certificar-se de chamar um recurso não entra em conflito com operações críticas em seu computador, considere agendar previsões durante o horário comercial.

## <a name="retrieving-capability-results"></a>Recuperando resultados de capacidade
Depois que um recurso foi invocado, os resultados mais recentes são visíveis usando **Get-InsightsCapability** ou **Get-InsightsCapabilityResult**. Esses cmdlets de saída mais recente **Status** e **descrição do Status** de cada recurso, que descrevem o resultado de cada previsão. O **Status** e **descrição do Status** campos são descritos na [documentos de recursos de compreensão](understanding-capabilities.md). 

Além disso, você pode usar o **Get-InsightsCapabilityResult** cmdlet para exibir os últimos resultados da 30 previsão e para recuperar os dados associados com a previsão: 

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
A extensão do sistema Insights automaticamente mostra o histórico de previsão e analisa os resultados do resultado JSON, dando a você um gráfico intuitivo, de alta fidelidade de cada previsão:

![Página de recurso único mostrando um gráfico de previsão e o histórico de previsão](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>Usando o log de eventos para recuperar resultados de capacidade
Informações do sistema registra um evento sempre que um recurso de conclusão de uma previsão. Esses eventos são visíveis na **Microsoft-Windows-System-Insights/Admin** channel e Insights de sistema publica uma ID de evento diferente para cada status:   

| Status de previsão | ID de evento |
| --------------- | --------------- |
| OK | 151 |
| Aviso | 148 |
| Crítico | 150 |
| Erro | 149 |
| Nenhuma | 132 |

>[!TIP]
>Use [do Azure Monitor](https://azure.microsoft.com/services/monitor/) ou [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) para agregar esses eventos e ver os resultados de previsão em um grupo de computadores.


## <a name="setting-a-capability-schedule"></a>Definir um agendamento de recurso
Além das previsões de demanda, você pode configurar previsões periódicas para cada recurso para que o recurso especificado é invocado automaticamente em um cronograma predefinido. Use o **Get-InsightsCapabilitySchedule** cmdlet para ver as agendas de recurso: 

>[!TIP]
>Use o operador de pipeline do PowerShell para ver informações para todos os recursos retornados pela **Get-InsightsCapability** cmdlet.

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

Previsões periódicas são habilitadas por padrão, embora eles podem ser desabilitados a qualquer momento usando o **Enable-InsightsCapabilitySchedule** e **Disable InsightsCapabilitySchedule** cmdlets:

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

Cada funcionalidade padrão está agendada para executar todos os dias às 3:00. No entanto, você pode criar agendas personalizadas para cada recurso, e o sistema Insights dá suporte a uma variedade de tipos de agenda, o que pode ser configurada usando o **InsightsCapabilitySchedule conjunto** cmdlet: 

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30 
```
>[!NOTE]
>Porque os recursos padrão analisar dados diários, recomendou usar agendas diárias para esses recursos. Saiba mais sobre os recursos padrão [aqui](understanding-capabilities.md).

Você também pode usar o Windows Admin Center para exibir e definir agendas para cada recurso clicando **configurações**. A agenda atual é mostrada na **agendamento** guia e você pode usar as ferramentas de GUI para criar uma nova agenda:

![Página de configurações mostrando atual agenda](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>Criação de ações de correção
Sistema Insights permite disparar scripts de correções personalizadas com base no resultado de um recurso. Para cada recurso, você pode configurar um script do PowerShell personalizado para cada status de previsão, permitindo que os administradores tomem ações corretivas automaticamente, em vez de exigir intervenção manual. 

Ações de correção de exemplo incluem a limpeza de disco em execução, estender um volume, executando a eliminação de duplicação, ao vivo, migração de VMs e a configuração de sincronização de arquivos do Azure.

Você pode ver as ações para cada recurso usando o **Get-InsightsCapabilityAction** cmdlet:

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

Você pode criar novas ações ou excluir ações existentes usando o **InsightsCapabilityAction conjunto** e **InsightsCapabilityAction remover** cmdlets. Cada ação é executada usando as credenciais que são especificadas na **ActionCredential** parâmetro.

>[!NOTE]
>Na versão inicial do sistema Insights, você deve especificar scripts de correções fora de diretórios de usuário. Isso será corrigido em uma versão futura.

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

Você também pode usar o Windows Admin Center para definir ações de correção usando o **ações** guia dentro de **configurações** página:

![Página de configurações em que o usuário pode especificar ações de correção](media/actions-page-contoso.png)


## <a name="see-also"></a>Consulte também
Para saber mais sobre o sistema Insights, use os seguintes recursos:

- [Visão geral de informações do sistema](overview.md)
- [Recursos de compreensão](understanding-capabilities.md)
- [Adicionando e desenvolvimento de recursos](adding-and-developing-capabilities.md)
- [Perguntas Frequentes de Insights de sistema](faq.md)