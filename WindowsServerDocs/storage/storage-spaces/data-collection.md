---
title: Coletar dados de diagnóstico com espaços de armazenamento diretos
description: Noções básicas sobre espaços diretos dados coleção de ferramentas de armazenamento, com exemplos específicos de como executar e usá-los.
keywords: Espaços de armazenamento, a coleta de dados, solução de problemas, canais de evento, Get-SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: 51cf96fb462b68f2ba01d49642a858430c71e9f5
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469609"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>Coletar dados de diagnóstico com espaços de armazenamento diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Há várias ferramentas de diagnóstico que podem ser usadas para coletar os dados necessários para solucionar problemas de espaços de armazenamento diretos e Cluster de Failover. Neste artigo, nos concentraremos em **Get-SDDCDiagnosticInfo** – uma ferramenta de um toque que coletará todas as informações relevantes para ajudá-lo a diagnosticar seu cluster.

Considerando que os logs e outras informações que **Get-SDDCDiagnosticInfo** são denso, as informações sobre como solucionar problemas apresentados a seguir serão úteis para solução de problemas avançada de problemas que foram escalados e que talvez exigir dados a serem enviados à Microsoft para triagem.

## <a name="installing-get-sddcdiagnosticinfo"></a>Instalando o Get-SDDCDiagnosticInfo

O **Get-SDDCDiagnosticInfo** cmdlet do PowerShell (também conhecido como **Get-PCStorageDiagnosticInfo**, anteriormente conhecido como **StorageHealth teste**) pode ser usado para coletar logs e executar verificações de integridade para Clustering de Failover (Cluster, recursos, redes, nós), (espaços de armazenamento Discos físicos, compartimentos, discos virtuais), eliminação de duplicação, compartilhamentos de arquivos SMB e Volumes Compartilhados do Cluster. 

Há dois métodos de instalação do script, que são estruturas de tópicos abaixo.

### <a name="powershell-gallery"></a>Galeria do PowerShell

O [da Galeria do PowerShell](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo) é um instantâneo do repositório GitHub. Observe que a instalação de itens da Galeria do PowerShell requer a versão mais recente do módulo PowerShellGet, que está disponível no Windows 10, no Windows Management Framework (WMF) 5.0 ou no instalador baseado em MSI (para PowerShell 3 e 4).

Você pode instalar o módulo executando o seguinte comando no PowerShell com privilégios de administrador:

``` PowerShell
Install-PackageProvider NuGet -Force
Install-Module PrivateCloud.DiagnosticInfo -Force
Import-Module PrivateCloud.DiagnosticInfo -Force
```

Para atualizar o módulo, execute o seguinte comando no PowerShell:

``` PowerShell
Update-Module PrivateCloud.DiagnosticInfo
```

### <a name="github"></a>GitHub

O [repositório GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/) é a versão mais recente do módulo, já que estamos continuamente estiver iterando aqui. Para instalar o módulo do GitHub, baixe o módulo mais recente a partir o [arquivamento](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip) e extraia o diretório PrivateCloud.DiagnosticInfo para o caminho correto de módulos do PowerShell apontado pelo ```$env:PSModulePath```

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

Se você precisar obter este módulo em um cluster offline, baixe o zip, mova-o para o nó do servidor de destino e instale o módulo.

## <a name="gathering-logs"></a>Coletando Logs

Depois que você tiver habilitado a canais de evento e concluir o processo de instalação, você pode usar o cmdlet do PowerShell Get-SDDCDiagnosticInfo no módulo para obter:

- Relatórios de integridade do armazenamento, além de obter detalhes sobre os componentes não íntegro
- Relatórios de capacidade de armazenamento por pool, o volume e o volume com eliminação de duplicação
- Logs de eventos de todos os nós de cluster e um relatório de resumo de erro

Suponha que seu cluster de armazenamento tem o nome *"CLUS01".*

Para executar em um cluster de armazenamento remoto:

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

Para executar localmente no nó de armazenamento em cluster:

``` PowerShell
Get-SDDCDiagnosticInfo
```

Para salvar os resultados em uma pasta especificada:

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

Aqui está um exemplo de como isso acontece em um cluster real:

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

Como você pode ver, o script também fará a validação do estado atual do cluster

![tela de powershell de coleta de dados](media/data-collection/CollectData.png)

Como você pode ver, todos os dados estão sendo gravados em pasta SDDCDiagTemp

![dados na captura de tela de Gerenciador de arquivo](media/data-collection/CollectDataFolder.png)

Depois que o script será concluído, ele criará o ZIP em seu diretório de usuários

![dados zip na captura de tela do powershell](media/data-collection/CollectDataResult.png)

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

Para referência, aqui está um link para o [relatório de exemplo](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt) e [zip de exemplo](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP).

Para exibir isso no Windows Admin Center (versão 1812 em diante), navegue até a *diagnóstico* guia. Como você ver na captura de tela abaixo, você pode: 

- Instalar as ferramentas de diagnóstico
- Atualizá-los (se estiverem desatualizadas), 
- Agendar execuções diárias de diagnóstico (elas têm um baixo impacto em seu sistema, normalmente levam < 5 minutos no segundo plano e não ocupam mais de 500mb em seu cluster)
- Modo de exibição anteriormente coletadas informações de diagnóstico se você precisar dar a ele a oferecer suporte ou analisá-los.

![captura de tela de diagnóstico wac](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>Saída de Get-SDDCDiagnosticInfo

A seguir estão os arquivos incluídos na saída de Get-SDDCDiagnosticInfo compactada.

### <a name="health-summary-report"></a>Relatório de resumo de integridade

O relatório de resumo de integridade é salvo como:
- 0_CloudHealthSummary.log

Esse arquivo é gerado depois de analisar todos os dados coletados e visa fornecer um rápido resumo do seu sistema. Ele contém:

- Informações do sistema
- Visão geral de integridade do armazenamento (número de nós para cima, recursos online, volumes compartilhados do cluster online, os componentes não íntegro, etc.)
- Obter detalhes sobre os componentes não íntegro (recursos de cluster que estão offline, com falha ou pendente online)
- Informações de firmware e driver
- Pool de disco físico e detalhes de volume
- Desempenho de armazenamento (desempenho de contadores são coletados)

Este relatório está sendo atualizado continuamente para incluir as informações mais úteis. Para obter informações mais recentes, consulte o [README GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md).

### <a name="logs-and-xml-files"></a>Logs e arquivos XML

O script é executado o log de vários scripts de coleta e salva a saída como arquivos xml. Podemos coletar logs de cluster e a integridade, informações do sistema (MSInfo32), logs de eventos não filtrados (clustering de failover, o diagnóstico de dis, hyper-v, espaços de armazenamento e muito mais) e informações de diagnóstico de armazenamento (logs operacionais). Para as últimas informações sobre quais informações são coletadas, consulte a [README GitHub (o que podemos coletar)](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include).

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>Como consumir os arquivos XML de Get-PCStorageDiagnosticInfo
Você pode consumir os dados dos arquivos XML fornecidos nos dados coletados pelo **Get-PCStorageDiagnosticInfo** cmdlet. Esses arquivos têm informações sobre o virtual disks, discos físicos, informações de cluster básico e outro PowerShell relacionados ao saídas. 

Para ver os resultados dessas saídas, abra uma janela do PowerShell e execute as etapas a seguir. 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>O que esperar a seguir?
Muitas melhorias e novos cmdlets para analisar a integridade do sistema SDDC.
Fornecer comentários sobre o que você deseja ver apresentando questões [aqui](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues). Além disso, fique à vontade contribuir com alterações úteis para o script, enviando uma [solicitação de pull](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls).
