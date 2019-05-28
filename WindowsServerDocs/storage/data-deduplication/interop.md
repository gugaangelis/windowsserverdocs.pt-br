---
ms.assetid: 60fca6b2-f1c0-451f-858f-2f6ab350d220
title: Interoperabilidade de Eliminação de Duplicação de Dados
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/16/2016
ms.openlocfilehash: 9453811b0f76b249c245990293ba82cf5a6e0867
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624634"
---
# <a name="data-deduplication-interoperability"></a>Interoperabilidade de Eliminação de Duplicação de Dados

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server de 2019

## <a id="supported"></a>Com suporte

### <a id="supported-ReFS"></a>ReFS
Eliminação de duplicação de dados dá suporte a partir do Windows Server 2019. 

### <a id="supported-clusters"></a>Clustering de failover

O [Clustering de failover](../..//failover-clustering/failover-clustering-overview.md) terá suporte completo se cada nó no cluster tiver o [recurso de Eliminação de Duplicação de Dados instalado](install-enable.md#install-dedup). Outras observações importantes:

* [Trabalhos de Eliminação de Duplicação de Dados iniciados manualmente](run.md#running-dedup-jobs-manually) devem ser executados no nó proprietário para o Volume Compartilhado Clusterizado.
* Trabalhos de Eliminação de Duplicação de Dados agendados são armazenados na tarefa do cluster agendada de modo que, se um volume com eliminação de duplicação for controlado por outro nó, o trabalho agendado será aplicado no próximo intervalo agendado.
* A Eliminação de Duplicação de Dados opera completamente em conjunto com o recurso [Atualização sem interrupção do SO do cluster](../..//failover-clustering/cluster-operating-system-rolling-upgrade.md).
* A Eliminação de Duplicação de Dados é totalmente compatível em volumes formatados com NTFS (espelhamento ou paridade) nos [Espaços de Armazenamento Diretos](../storage-spaces/storage-spaces-direct-overview.md). A eliminação de duplicação não tem suporte em volumes com várias camadas. Consulte [Eliminação de Duplicação de Dados no ReFS](interop.md#unsupported-refs) para obter mais informações.

### <a id="supported-storage-replica"></a>Réplica de armazenamento
[Réplica de Armazenamento](../storage-replica/storage-replica-overview.md) é totalmente compatível. A eliminação de duplicação de dados deve ser configurada para não ser executada na cópia secundária.

### <a id="supported-branchcache"></a>BranchCache
Você pode otimizar o acesso a dados pela rede habilitando [BranchCache](../../networking/branchcache/branchcache.md) em servidores e clientes. Quando um sistema habilitado para BranchCache se comunica por uma WAN com um servidor de arquivos remoto que está executando a eliminação de duplicação de dados, todos os arquivos com duplicação já estão indexados e com hash. Portanto, as solicitações de dados de uma filial são rapidamente calculadas. Isso é similar a pré-indexar ou colocar previamente em hash um servidor habilitado para BranchCache.

### <a id="supported-dfsr"></a>Replicação do DFS
A Eliminação de Duplicação de Dados funciona com a replicação DFS (Sistema de Arquivos Distribuído). A otimização ou cancelamento da otimização de um arquivo não vai disparar uma replicação porque o arquivo não é alterado. A Replicação do DFS usa RDC (Compactação Diferencial Remota), não as partes no repositório de partes, com economia de uso de rede. Os arquivos na réplica também podem ser otimizados com eliminação de duplicação se a réplica estiver usando Eliminação de Duplicação de Dados.

### <a id="supported-quotas"></a>Quotas
A Eliminação de Duplicação de Dados não dá suporte à criação de uma cota fixa em uma pasta raiz de volume que também tem a eliminação de duplicação habilitada. Quando há uma cota rígida em uma raiz de volume, o espaço livre real no volume e o espaço restrito à cota no volume são os mesmos. Isso pode provocar falha nos trabalhos de eliminação de duplicação. No entanto, é possível criar uma cota flexível em uma raiz de volume que tenha a eliminação de duplicação habilitada. 

Quando a cota é habilitada em um volume com eliminação de duplicação, a cota usa o tamanho lógico do arquivo em vez do tamanho físico do arquivo. O uso de cota (incluindo qualquer limite de cota) não é alterado quando um arquivo é processado pela eliminação de duplicação. Todas as demais funcionalidades de cota, incluindo cotas flexíveis de raiz de volume e cotas em subpastas, funcionam normalmente durante a eliminação de duplicação.

### <a id="supported-windows-server-backup"></a>Backup do Windows Server
O Backup do Windows Server pode fazer backup de um volume otimizado "como está" (ou seja, sem remover dados com eliminação de duplicação). As etapas a seguir mostram como fazer backup de um volume e como restaurar um volume ou arquivos selecionados de um volume.
1. Instale o Backup do Windows Server.  
    ```PowerShell
    Install-WindowsFeature -Name Windows-Server-Backup
    ```

2. Faça backup do volume E: de volume para outro volume, executando o seguinte, substituindo os nomes do volume correto de acordo com a situação.  
    ```PowerShell
    wbadmin start backup –include:E: -backuptarget:F: -quiet
    ```
3. Obtenha a ID de versão do backup que você acabou de criar.

    ```PowerShell
    wbadmin get versions
    ```

    Essa ID de versão de saída será uma cadeia de caracteres de data e hora, por exemplo: 08/18/2016-06:22.

4. Restaure o volume inteiro.
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:Volume  -items:E: -recoveryTarget:E:
    ```

    **-OU-**  

    Restaure uma pasta específica (nesse caso, a pasta E:\Docs):
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:File  -items:E:\Docs  -recursive
    ```

## <a id="unsupported"></a>Sem suporte

### <a id="unsupported-windows-client"></a>Windows 10 (client OS)
Não há suporte para a eliminação da duplicação de dados no Windows 10. Há várias postagens de blog populares na Comunidade do Windows que descrevem como remover os binários do Windows Server 2016 e instalar no Windows 10, mas esse cenário não foi validado como parte do desenvolvimento de eliminação de duplicação de dados. [Vote nesse item para o Windows 10 no Windows Server Storage UserVoice](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/9011008-add-deduplication-support-to-client-os).

### <a id="unsupported-windows-search"></a>Windows Search
O Windows Search não dá suporte à Eliminação de Duplicação de Dados. A Eliminação de Duplicação de Dados usa pontos de nova análise, que o Windows Search não pode indexar. Portanto, o Windows Search ignora todos os arquivos com eliminação de duplicação, excluindo-os do índice. Como resultado, resultados da pesquisa podem ficar incompletos para volumes com eliminação de duplicação. [Vote nesse item para o Windows Server vNext no Windows Server Storage UserVoice](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/17888647-make-windows-search-service-work-with-data-dedupli).

### <a id="unsupported-robocopy"></a>Robocopy
A execução de Robocopy com Eliminação de Duplicação de Dados não é recomendada porque determinados comandos do Robocopy podem corromper o repositório de partes. O Repositório de partes é armazenado na pasta Informações do Volume do Sistema para um volume. Se a pasta é excluída, os arquivos otimizados (pontos de nova análise) que são copiados do volume de origem ficam corrompidos porque os fragmentos de dados não são copiados para o volume de destino.
