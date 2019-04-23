---
title: Solucionar problemas de desempenho do Gerenciador de memória e Cache
description: Solucionar problemas de desempenho do Gerenciador de memória no Windows Server 16 e de Cache
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 66c7e2a6b264a837c65df927b271fadd2672fa24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835787"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Solucionar problemas de desempenho do Gerenciador de memória e Cache

Antes do Windows Server 2012, dois problemas principais de potencias causado o cache de arquivos do sistema a crescer até que a memória disponível foi quase esgotada em determinadas cargas de trabalho. Quando resultados essa situação no sistema que está sendo lento, você pode determinar se o servidor está enfrentando um desses problemas.


## <a name="counters-to-monitor"></a>Contadores para monitorar

-   Memória\\em longo prazo em espera Cache de tempo de vida médio (s) &lt; 1800 segundos

-   Memória\\megabytes disponíveis está baixo

-   Memória\\Bytes residentes de Cache do sistema

Se memória\\megabytes disponíveis está baixo e, ao mesmo tempo memória\\Bytes de residentes de Cache do sistema está consumindo uma parte significativa da memória física, você pode usar [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) para descobrir o que o cache está sendo usado. para.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>Cache de arquivo do sistema contém estruturas de dados do metarquivo NTFS


Esse problema é indicado por um número muito alto de páginas ativas do metarquivo na RAMMAP de saída, conforme mostrado na figura a seguir. Esse problema pode foram observado nos servidores ocupados com milhões de arquivos que está sendo acessados, assim, resultando no cache de dados do metarquivo NTFS não está sendo liberados do cache.

![modo de exibição rammap](../../media/perftune-guide-rammap.png)

O problema costumava ser reduzidos *DynCache* ferramenta. No Windows Server 2012 +, a arquitetura foi reprojetada e esse problema não deve existir.

## <a name="system-file-cache-contains-memory-mapped-files"></a>Cache de arquivo do sistema contém arquivos mapeados na memória


Esse problema é indicado por um número muito alto de páginas Active Directory de arquivo mapeado na saída RAMMAP. Isso geralmente indica que algum aplicativo no servidor está abrindo muitos arquivos grandes usando [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) API com o arquivo\_sinalizador\_RANDOM\_acesso sinalizador definido.

Esse problema é descrito detalhadamente no artigo KB [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369). ARQUIVO\_sinalizador\_RANDOM\_sinalizador de acesso é uma dica para o Gerenciador de Cache para manter exibições mapeadas do arquivo na memória, desde que possíveis (até que o Gerenciador de memória não sinalizar a condição de memória insuficiente). Ao mesmo tempo, esse sinalizador instrui o Gerenciador de Cache para desabilitar a pré-busca de dados de arquivo.

Essa situação foi abrandada até certo ponto por aprimoramentos de corte do conjunto de trabalhar no Windows Server 2012 +, mas o problema em si precisa ser resolvida principalmente pelo fornecedor do aplicativo usando o arquivo não\_sinalizador\_RANDOM\_ACCESS. Uma solução alternativa para o fornecedor do aplicativo pode ser usar prioridade de memória insuficiente ao acessar os arquivos. Isso pode ser feito usando o [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) API. Páginas que são acessadas na prioridade de memória baixo são removidas do conjunto de forma mais agressiva de trabalho.

Gerenciador de cache, começando no Windows Server 2016 ainda mais Isso atenua ignorando FILE_FLAG_RANDOM_ACCESS ao tomar decisões de filtragem, portanto, ele será tratado como qualquer outro arquivo aberto sem o sinalizador FILE_FLAG_RANDOM_ACCESS (Gerenciador de Cache ainda honra isso Sinalizador para desabilitar a pré-busca de dados de arquivo). Você ainda pode causar sobrecarga de cache do sistema se você tiver um número grande de arquivos abertos com esse sinalizador e acessados de modo verdadeiramente aleatório. É altamente recomendável que FILE_FLAG_RANDOM_ACCESS não usada por aplicativos.
