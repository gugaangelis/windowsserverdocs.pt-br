---
title: Gerenciamento de recursos
description: O System insights expõe uma variedade de configurações que podem ser configuradas para cada funcionalidade, e essas configurações podem ser ajustadas para atender às necessidades específicas de sua implantação. Este tópico descreve como gerenciar as várias configurações de cada funcionalidade por meio do centro de administração do Windows ou do PowerShell, fornecendo exemplos básicos do PowerShell e capturas de tela do centro de administração do Windows para demonstrar como ajustar essas configurações.
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 3f4e80136b3c70b7a121663a6defa048d2b0e852
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766219"
---
# <a name="managing-capabilities"></a>Gerenciamento de recursos

>Aplica-se a: Windows Server 2019

No Windows Server 2019, o System insights expõe uma variedade de configurações que podem ser configuradas para cada funcionalidade, e essas configurações podem ser ajustadas para atender às necessidades específicas de sua implantação. Este tópico descreve como gerenciar as várias configurações de cada funcionalidade por meio do centro de administração do Windows ou do PowerShell, fornecendo exemplos básicos do PowerShell e capturas de tela do centro de administração do Windows para demonstrar como ajustar essas configurações.

>[!TIP]
>Você também pode usar esses vídeos curtos para ajudá-lo a começar e a gerenciar com segurança o System insights: [introdução ao System insights em 10 minutos](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

Embora esta seção Forneça exemplos do PowerShell, você pode usar a [documentação do PowerShell do System insights](/powershell/module/systeminsights/) para ver todos os cmdlets, parâmetros e conjuntos de parâmetros no System insights.

## <a name="viewing-capabilities"></a>Recursos de exibição

Para começar, você pode listar todos os recursos disponíveis usando o cmdlet **Get-InsightsCapability** :

```PowerShell
Get-InsightsCapability
```
Esses recursos também estão visíveis na extensão do System insights:

![Página de visão geral da lista de recursos disponíveis do System insights](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>Habilitando e desabilitando um recurso
Cada recurso pode ser habilitado ou desabilitado. Desabilitar uma funcionalidade impede que a capacidade seja invocada e, para recursos não padrão, desabilitar uma funcionalidade interrompe toda a coleta de dados para esse recurso. Por padrão, todos os recursos estão habilitados e você pode verificar o estado de um recurso usando o cmdlet **Get-InsightsCapability** .

Para habilitar ou desabilitar um recurso, use os cmdlets **Enable-InsightsCapability** e **Disable-InsightsCapability** :

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
```
Essas configurações também podem ser alternadas selecionando um recurso no centro de administração do Windows clicando nos botões **habilitar** ou **desabilitar** .

### <a name="invoking-a-capability"></a>Invocando um recurso
Invocar uma funcionalidade imediatamente executa a capacidade de recuperar uma previsão, e os administradores podem invocar um recurso a qualquer momento clicando no botão **invocar** no centro de administração do Windows ou usando o cmdlet **Invoke-InsightsCapability** :

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>Para certificar-se de que a invocação de uma funcionalidade não entre em conflito com operações críticas em seu computador, considere o agendamento de previsões fora do horário comercial.

## <a name="retrieving-capability-results"></a>Recuperando os resultados da funcionalidade
Depois que um recurso é invocado, os resultados mais recentes são visíveis usando **Get-InsightsCapability** ou **Get-InsightsCapabilityResult**. Esses cmdlets geram a **Descrição** de **status** e status mais recente de cada funcionalidade, que descrevem o resultado de cada previsão. Os campos de **Descrição** **status** e status são mais descritos no [documento noções básicas sobre recursos](understanding-capabilities.md).

Além disso, você pode usar o cmdlet **Get-InsightsCapabilityResult** para exibir os 30 últimos resultados de previsão e para recuperar os dados associados à previsão:

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
A extensão de informações do sistema mostra automaticamente o histórico de previsão e analisa os resultados do resultado JSON, oferecendo a você um grafo intuitivo e de alta fidelidade de cada previsão:

![Página de recurso único mostrando um grafo de previsão e o histórico de previsão](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>Usando o log de eventos para recuperar os resultados da funcionalidade
O System insights registra um evento sempre que um recurso termina uma previsão. Esses eventos são visíveis no canal **Microsoft-Windows-System-insights/admin** e o System insights publica uma ID de evento diferente para cada status:

| Status de previsão | ID do evento |
| --------------- | --------------- |
| Ok | 151 |
| Aviso | 148 |
| Crítico | 150 |
| Erro | 149 |
| Nenhum | 132 |

>[!TIP]
>Use [Azure monitor](https://azure.microsoft.com/services/monitor/) ou [System Center Operations Manager](/system-center/scom/welcome?view=sc-om-1807) para agregar esses eventos e ver os resultados da previsão em um grupo de computadores.


## <a name="setting-a-capability-schedule"></a>Configurando um agendamento de funcionalidade
Além das previsões sob demanda, você pode configurar previsões periódicas para cada funcionalidade para que o recurso especificado seja automaticamente invocado em um agendamento predefinido. Use o cmdlet **Get-InsightsCapabilitySchedule** para ver os cronogramas de funcionalidade:

>[!TIP]
>Use o operador de pipeline no PowerShell para ver informações de todos os recursos retornados pelo cmdlet **Get-InsightsCapability** .

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

As previsões periódicas são habilitadas por padrão, embora possam ser desabilitadas a qualquer momento usando os cmdlets **Enable-InsightsCapabilitySchedule** e **Disable-InsightsCapabilitySchedule** :

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

Cada recurso padrão é agendado para ser executado todos os dias às 3am. No entanto, você pode criar agendas personalizadas para cada funcionalidade e o System insights dá suporte a uma variedade de tipos de agendamento, que podem ser configurados usando o cmdlet **set-InsightsCapabilitySchedule** :

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30
```
>[!NOTE]
>Como os recursos padrão analisam dados diários, é recomendável usar agendas diárias para esses recursos. Saiba mais sobre os recursos padrão [aqui](understanding-capabilities.md).

Você também pode usar o centro de administração do Windows para exibir e definir agendas para cada recurso clicando em **configurações**. A agenda atual é mostrada na guia **agenda** , e você pode usar as ferramentas de GUI para criar uma nova agenda:

![Página de configurações mostrando a agenda atual](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>Criando ações de correção
O System insights permite iniciar scripts de correção personalizados com base no resultado de um recurso. Para cada funcionalidade, você pode configurar um script personalizado do PowerShell para cada status de previsão, permitindo que os administradores adotem a ação corretiva automaticamente, em vez de exigir intervenção manual.

As ações de correção de exemplo incluem a execução de limpeza de disco, a extensão de um volume, a execução de eliminação de duplicação, a migração dinâmica de VMs e a configuração de Sincronização de Arquivos do Azure

Você pode ver as ações para cada funcionalidade usando o cmdlet **Get-InsightsCapabilityAction** :

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

Você pode criar novas ações ou excluir ações existentes usando os cmdlets **set-InsightsCapabilityAction** e **Remove-InsightsCapabilityAction** . Cada ação é executada usando as credenciais especificadas no parâmetro **ActionCredential** .

>[!NOTE]
>Na versão inicial do System insights, você deve especificar os scripts de correção fora dos diretórios do usuário. Isso será corrigido em uma versão futura.

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

Você também pode usar o centro de administração do Windows para definir ações de correção usando a guia **ações** na página **configurações** :

![Página de configurações onde o usuário pode especificar ações de correção](media/actions-page-contoso.png)


## <a name="additional-references"></a>Referências adicionais
Para saber mais sobre o System insights, use os seguintes recursos:

- [Visão geral dos insights do sistema](overview.md)
- [Noções básicas dos recursos](understanding-capabilities.md)
- [Adicionar e desenvolver recursos](adding-and-developing-capabilities.md)
- [Perguntas frequentes do System insights](faq.md)