---
title: Solucionar problemas de desempenho do Gerenciador de memória e cache
description: Solucionar problemas de desempenho do Gerenciador de memória e cache no Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: pavel; atales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0b01808564cfaf1eaedf30a66c774e2228205847
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851649"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Solucionar problemas de desempenho do Gerenciador de memória e cache

Antes do Windows Server 2012, dois problemas potenciais principais causaram o aumento do cache de arquivos do sistema até que a memória disponível estivesse quase esgotada em determinadas cargas de trabalho. Quando essa situação resulta na lenta do sistema, você pode determinar se o servidor está enfrentando um desses problemas.


## <a name="counters-to-monitor"></a>Contadores a serem monitorados

-   Memória\\tempo de vida médio (s) de cache em espera de longo prazo &lt; 1800 segundos

-   A memória\\MBytes disponíveis está baixa

-   Memória\\bytes residentes de cache do sistema

Se a memória\\MBytes disponíveis estiver baixa e, ao mesmo tempo, a memória\\bytes residentes do cache do sistema estiverem consumindo uma parte significativa da memória física, você poderá usar o [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) para descobrir a que o cache está sendo usado.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>O cache de arquivos do sistema contém estruturas de dados do metarquivo NTFS


Esse problema é indicado por um número muito alto de páginas de metarquivo ativas na saída do RAMMAP, conforme mostrado na figura a seguir. Esse problema pode ter sido observado em servidores ocupados com milhões de arquivos sendo acessados, resultando assim em armazenar em cache os dados do metarquivo NTFS que não estão sendo liberados do cache.

![exibição de RAMMap](../../media/perftune-guide-rammap.png)

O problema costumava ser mitigado pela ferramenta *DynCache* . No Windows Server 2012 +, a arquitetura foi reprojetada e esse problema não deve mais existir.

## <a name="system-file-cache-contains-memory-mapped-files"></a>O cache de arquivos do sistema contém arquivos de memória mapeada


Esse problema é indicado por um número muito alto de páginas de arquivos mapeadas ativas na saída do RAMMAP. Isso geralmente indica que algum aplicativo no servidor está abrindo muitos arquivos grandes usando a API [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) com o sinalizador de\_de arquivo\_conjunto de sinalizadores de acesso\_aleatório.

Esse problema é descrito em detalhes no artigo [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369)da base de conhecimento. O sinalizador de\_de arquivo\_sinalizador de acesso\_aleatório é uma dica para o Gerenciador de cache manter as exibições mapeadas do arquivo na memória o mais longo possível (até que o Gerenciador de memória não sinalize a condição de memória insuficiente). Ao mesmo tempo, esse sinalizador instrui o Gerenciador de cache a desabilitar a pré-busca de dados de arquivo.

Essa situação foi atenuada em certo ponto ao trabalhar com os aprimoramentos de corte de conjunto de trabalho no Windows Server 2012 +, mas o problema em si precisa ser abordado principalmente pelo fornecedor do aplicativo, não usando o sinalizador de\_de arquivo\_acesso\_aleatório. Uma solução alternativa para o fornecedor do aplicativo pode ser usar baixa prioridade de memória ao acessar os arquivos. Isso pode ser feito usando a API [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) . As páginas que são acessadas com baixa prioridade de memória são removidas do conjunto de trabalho de forma mais agressiva.

O Gerenciador de cache, a partir do Windows Server 2016, reduz ainda mais isso ignorando FILE_FLAG_RANDOM_ACCESS ao fazer decisões de corte, portanto, ele é tratado da mesma forma que qualquer outro arquivo aberto sem o sinalizador FILE_FLAG_RANDOM_ACCESS (o Gerenciador de cache ainda respeita esse sinalizador para desabilitar a pré-busca de dados de arquivo). Você ainda poderá causar o inchar no cache do sistema se tiver um grande número de arquivos abertos com esse sinalizador e acessado de maneira verdadeiramente aleatória. É altamente recomendável que não FILE_FLAG_RANDOM_ACCESS ser usado por aplicativos.
