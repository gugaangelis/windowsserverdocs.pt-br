---
title: Aprimoramentos do Gerenciador de memória e cache
description: Aprimoramentos de cache e Gerenciador de Memória no Windows Server 2016
ms.topic: article
ms.author: pavel; atales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: c2fdceb7ff3743890c73ee4108ffcf8e00fe437f
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992057"
---
# <a name="cache-and-memory-manager-improvements"></a>Aprimoramentos do Gerenciador de memória e cache

Este tópico descreve o Gerenciador de cache e aprimoramentos do Gerenciador de memória no Windows Server 2012 e 2016.

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Aprimoramentos do Gerenciador de cache no Windows Server 2016
O Gerenciador de cache também adicionou suporte para leituras reais comsíncronas em cache.
Isso poderia potencialmente melhorar o desempenho de um aplicativo se ele depender muito de leituras em cache assíncronas.Embora a maioria dos sistemas de filebox tenha suporte para leituras em cache assíncronos por um tempo, muitas vezes houve limitações de desempenho devido a várias opções de design relacionadas à manipulação de filas de trabalho internas de pools de threads e de sistemas de cache.Com suporte do kernel-adequado, o Gerenciador de cache agora oculta todas as complexidades do pool de threads e do gerenciamento da fila de trabalho dos sistemas de File, tornando-a mais eficiente no tratamento de leituras armazenadas em cache assíncronas. O Gerenciador de cache tem um conjunto de estruturas de dados de controle para cada um dos níveis de aninhamento de VHD (máximo de suporte do sistema) para maximizar o paralelismo.


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Aprimoramentos do Gerenciador de cache no Windows Server 2012
Além dos aprimoramentos do Gerenciador de cache para ler a lógica antecipada de cargas de trabalho sequenciais, uma nova API [CcSetReadAheadGranularityEx](/windows-hardware/drivers/ifs/ccsetreadaheadgranularityex) foi adicionada para permitir que drivers de sistema de arquivos, como SMB, alterem seus parâmetros Read Ahead. Ele permite uma melhor taxa de transferência para cenários de arquivo remoto enviando várias solicitações Read Ahead de tamanho pequeno em vez de enviar uma única solicitação Read Ahead grande. Somente os componentes de kernel, como drivers de sistema de arquivos, podem configurar esses valores programaticamente por arquivo.

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Aprimoramentos do Gerenciador de memória no Windows Server 2012
Habilitar a combinação de páginas pode reduzir o uso de memória em servidores que têm muitas páginas particulares e paginável com conteúdo idêntico. Por exemplo, servidores que executam várias instâncias do mesmo aplicativo com uso intensivo de memória ou um único aplicativo que funciona com dados altamente repetitivos podem ser bons candidatos a experimentar a combinação de páginas. A desvantagem de habilitar a combinação de páginas é o aumento do uso da CPU.

Aqui estão alguns exemplos de funções de servidor em que a combinação de páginas é improvável de oferecer muito benefício:

-   Servidores de arquivos (a maior parte da memória é consumida por páginas de arquivos que não são privadas e, portanto, não podem ser combinadas)

-   Microsoft SQL Servers que são configurados para usar AWE ou páginas grandes (a maior parte da memória é privada, mas não paginável)

A combinação de páginas está desabilitada por padrão, mas pode ser habilitada usando o cmdlet [Enable-MMAgent](/powershell/module/mmagent/enable-mmagent?view=win10-ps) do Windows PowerShell. A combinação de páginas foi adicionada no Windows Server 2012.