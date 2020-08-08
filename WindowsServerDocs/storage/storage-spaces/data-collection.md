---
title: Coletar dados de diagnóstico com Espaços de Armazenamento Diretos
description: Entendendo Espaços de Armazenamento Diretos ferramentas de coleta de dados, com exemplos específicos de como executá-las e usá-las.
ms.author: adagashe
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.openlocfilehash: fa71408dbb6a4757150ee896a760f37914aacc38
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960969"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>Coletar dados de diagnóstico com Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Há várias ferramentas de diagnóstico que podem ser usadas para coletar os dados necessários para solucionar problemas de Espaços de Armazenamento Diretos e cluster de failover. Neste artigo, vamos nos concentrar no **Get-SDDCDiagnosticInfo** -uma ferramenta de toque que reunirá todas as informações relevantes para ajudá-lo a diagnosticar seu cluster.

Considerando que os logs e outras informações que **Get-SDDCDiagnosticInfo** são densas, as informações sobre solução de problemas apresentadas abaixo serão úteis para solucionar problemas avançados que foram escalonados e que podem exigir que os dados sejam enviados à Microsoft para fins de preparo.

## <a name="installing-get-sddcdiagnosticinfo"></a>Instalando o Get-SDDCDiagnosticInfo

O cmdlet **Get-SDDCDiagnosticInfo** do PowerShell (também conhecido como **Get-PCStorageDiagnosticInfo**, anteriormente conhecido como **Test-StorageHealth**), pode ser usado para reunir logs e realizar verificações de integridade para clustering de failover (cluster, recursos, redes, nós), espaços de armazenamento (discos físicos, compartimentos, discos virtuais), volumes compartilhados de cluster, compartilhamentos de arquivos SMB e eliminação de duplicação.

Há dois métodos de instalação do script, os quais são contornos abaixo.

### <a name="powershell-gallery"></a>Galeria do PowerShell

O [Galeria do PowerShell](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo) é um instantâneo do repositório github. Observe que a instalação de itens da Galeria do PowerShell requer a versão mais recente do módulo PowerShellGet, que está disponível no Windows 10, no WMF (Windows Management Framework) 5,0 ou no instalador baseado em MSI (para o PowerShell 3 e 4).

Instalamos a versão mais recente das [ferramentas de diagnóstico de rede da Microsoft](https://www.powershellgallery.com/packages/MSFT.Network.Diag) durante esse processo, pois o Get-SDDCDiagnosticInfo depende disso. Este módulo de manifesto contém a ferramenta de diagnóstico e solução de problemas de rede, que é mantida pelo grupo de produtos Microsoft Core Networking na Microsoft.

Você pode instalar o módulo executando o seguinte comando no PowerShell com privilégios de administrador:

``` PowerShell
Install-PackageProvider NuGet -Force
Install-Module PrivateCloud.DiagnosticInfo -Force
Import-Module PrivateCloud.DiagnosticInfo -Force
Install-Module -Name MSFT.Network.Diag
```

Para atualizar o módulo, execute o seguinte comando no PowerShell:

``` PowerShell
Update-Module PrivateCloud.DiagnosticInfo
```

### <a name="github"></a>GitHub

O [repositório GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/) é a versão mais atualizada do módulo, pois estamos continuamente Iterando aqui. Para instalar o módulo do GitHub, baixe o módulo mais recente do [arquivo](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip) e extraia o diretório nuvem particular. DiagnosticInfo para o caminho correto de módulos do PowerShell apontado por```$env:PSModulePath```

``` PowerShell
# Allowing Tls12 and Tls11 -- e.g. github now requires Tls12
# If this is not set, the Invoke-WebRequest fails with "The request was aborted: Could not create SSL/TLS secure channel."
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$module = 'PrivateCloud.DiagnosticInfo'
Invoke-WebRequest -Uri https://github.com/PowerShell/$module/archive/master.zip -OutFile $env:TEMP\master.zip
Expand-Archive -Path $env:TEMP\master.zip -DestinationPath $env:TEMP -Force
if (Test-Path $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module) {
    rm -Recurse $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module -ErrorAction Stop
    Remove-Module $module -ErrorAction SilentlyContinue
} else {
    Import-Module $module -ErrorAction SilentlyContinue
}
if (-not ($m = Get-Module $module -ErrorAction SilentlyContinue)) {
    $md = "$env:ProgramFiles\WindowsPowerShell\Modules"
} else {
    $md = (gi $m.ModuleBase -ErrorAction SilentlyContinue).PsParentPath
    Remove-Module $module -ErrorAction SilentlyContinue
    rm -Recurse $m.ModuleBase -ErrorAction Stop
}
cp -Recurse $env:TEMP\$module-master\$module $md -Force -ErrorAction Stop
rm -Recurse $env:TEMP\$module-master,$env:TEMP\master.zip
Import-Module $module -Force

```

Se você precisar obter esse módulo em um cluster offline, baixe o zip, mova-o para o nó do servidor de destino e instale o módulo.

## <a name="gathering-logs"></a>Coletando logs

Depois de habilitar os canais de eventos e concluir o processo de instalação, você poderá usar o cmdlet Get-SDDCDiagnosticInfo do PowerShell no módulo para obter:

- Relatórios sobre integridade de armazenamento, além de detalhes sobre componentes não íntegros
- Relatórios de capacidade de armazenamento por pool, volume e volume com eliminação de duplicação
- Logs de eventos de todos os nós de cluster e um relatório de erro de resumo

Suponha que o cluster de armazenamento tenha o nome *"CLUS01".*

Para executar em um cluster de armazenamento remoto:

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

Para executar localmente no nó de armazenamento clusterizado:

``` PowerShell
Get-SDDCDiagnosticInfo
```

Para salvar os resultados em uma pasta especificada:

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder
```

Veja um exemplo de como isso se parece em um cluster real:

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

Como você pode ver, o script também fará a validação do estado atual do cluster

![captura de tela do PowerShell de coleta de dados](media/data-collection/CollectData.png)

Como você pode ver, todos os dados estão sendo gravados na pasta SDDCDiagTemp

![dados na captura de tela do explorador de arquivos](media/data-collection/CollectDataFolder.png)

Depois que o script for concluído, ele criará o ZIP no diretório de usuários

![captura de tela data zip no PowerShell](media/data-collection/CollectDataResult.png)

Vamos gerar um relatório em um arquivo de texto

```PowerShell
#find the latest diagnostic zip in UserProfile
    $DiagZip=(get-childitem $env:USERPROFILE | where Name -like HealthTest*.zip)
    $LatestDiagPath=($DiagZip | sort lastwritetime | select -First 1).FullName
#expand to temp directory
    New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
    Expand-Archive -Path $LatestDiagPath -DestinationPath D:\SDDCDiagTemp -Force
#generate report and save to text file
    $report=Show-SddcDiagnosticReport -Path D:\SDDCDiagTemp
    $report | out-file d:\SDDCReport.txt

```

Para referência, aqui está um link para o [relatório de exemplo](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt) e o [zip de exemplo](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP).

Para exibir isso no centro de administração do Windows (versão 1812 em diante), navegue até a guia *diagnóstico* . Como você pode ver na captura de tela abaixo, é possível

- Instalar ferramentas de diagnóstico
- Atualizá-los (se estiverem desatualizados),
- Agendar execuções diárias de diagnóstico (elas têm um baixo impacto no sistema, geralmente levam <5 minutos em segundo plano e não ocupam mais do que 500 MB em seu cluster)
- Exiba informações de diagnóstico coletadas anteriormente se você precisar dar suporte ou analisá-las por conta própria.

![captura de tela do WAC Diagnostics](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>Saída Get-SDDCDiagnosticInfo

A seguir estão os arquivos incluídos na saída compactada de Get-SDDCDiagnosticInfo.

### <a name="health-summary-report"></a>Relatório de Resumo de integridade

O relatório de Resumo de integridade é salvo como:
- 0_CloudHealthSummary. log

Esse arquivo é gerado após a análise de todos os dados coletados e destina-se a fornecer um resumo rápido do seu sistema. Ele contém:

- Informações do sistema
- Visão geral da integridade do armazenamento (número de nós, recursos online, volumes compartilhados do cluster online, componentes não íntegros, etc.)
- Detalhes sobre componentes não íntegros (recursos de cluster que estão offline, com falha ou que estão online pendentes)
- Informações de firmware e driver
- Detalhes do pool, disco físico e volume
- Desempenho do armazenamento (contadores de desempenho são coletados)

Este relatório está sendo atualizado continuamente para incluir informações mais úteis. Para obter as informações mais recentes, consulte o [Leiame do GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md).

### <a name="logs-and-xml-files"></a>Logs e arquivos XML

O script executa vários scripts de coleta de logs e salva a saída como arquivos XML. Coletamos logs de cluster e de integridade, informações do sistema (MSInfo32), logs de eventos não filtrados (clustering de failover, desdiagnósticos, Hyper-v, espaços de armazenamento e muito mais) e informações de diagnóstico de armazenamento (logs operacionais). Para obter as informações mais recentes sobre quais informações são coletadas, consulte o [Leiame do GitHub (o que coletamos)](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include).

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>Como consumir os arquivos XML de Get-PCStorageDiagnosticInfo
Você pode consumir os dados dos arquivos XML fornecidos nos dados coletados pelo cmdlet **Get-PCStorageDiagnosticInfo** . Esses arquivos têm informações sobre discos virtuais, discos físicos, informações básicas de cluster e outras saídas relacionadas ao PowerShell.

Para ver os resultados dessas saídas, abra uma janela do PowerShell e execute as etapas a seguir.

```PowerShell
ipmo storage
$d = import-clixml <filename>
$d
```

## <a name="what-to-expect-next"></a>O que esperar em seguida?
Muitos aprimoramentos e novos cmdlets para analisar a integridade do sistema SDDC.
Forneça comentários sobre o que você gostaria de ver ao arquivar problemas [aqui](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues). Além disso, sinta-se à vontade para contribuir com alterações úteis no script, enviando uma [solicitação de pull](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls).
