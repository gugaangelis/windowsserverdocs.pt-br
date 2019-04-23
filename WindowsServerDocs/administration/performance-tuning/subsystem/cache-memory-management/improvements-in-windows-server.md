---
title: Aprimoramentos de Gerenciador de memória e cache
description: Cache e melhorias do Gerenciador de memória no Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bd15cfc0714d1992e6b15d716b6b96ce259786da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868577"
---
# <a name="cache-and-memory-manager-improvements"></a>Aprimoramentos de Gerenciador de memória e cache

Este tópico descreve os aprimoramentos do Gerenciador de Cache e o Gerenciador de memória no Windows Server 2012 e 2016.

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Melhorias do Gerenciador de cache no Windows Server 2016
Gerenciador de cache também adicionou suporte para true assíncronas armazenados em cache lê.
Potencialmente, isso pode melhorar o desempenho de um aplicativo se ele depende muito a leitura de cache assíncrona.  Embora a maioria dos sistemas de arquivos na caixa têm suporte para leituras em cache assíncrono por um tempo, havia muitas vezes as limitações de desempenho devido a várias opções de projeto relacionadas ao tratamento de pools de threads e filas de trabalho internas dos sistemas de arquivos.  Com o suporte do kernel adequada, Gerenciador de Cache agora oculta todo o pool de threads e as complexidades de gerenciamento de fila de trabalho de torná-la mais eficiente em manipulação assíncrona de sistemas de arquivos armazenados em cache leituras. Gerenciador de cache tem um conjunto de controle datastructures para cada um dos (máximo de sistema com suporte) níveis de aninhamento de VHD para maximizar o paralelismo.


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Melhorias do Gerenciador de cache no Windows Server 2012
Além dos aprimoramentos do Gerenciador de Cache de leitura antecipada lógica para cargas de trabalho sequenciais, uma nova API [CcSetReadAheadGranularityEx](https://msdn.microsoft.com/library/windows/hardware/hh406341.aspx) foi adicionada para permitir que drivers de sistema, como SMB de arquivos, alterar seus parâmetros de leitura antecipados. Ele permite que a melhor taxa de transferência para cenários de arquivo remoto, enviando que solicitações de leitura da pequenos vários com antecedência em vez de enviar a que solicitação antecipada de leitura de um único grande. Somente componentes de kernel, como drivers de sistema de arquivos, por meio de programação podem configurar esses valores em uma base por arquivo.

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Melhorias do Gerenciador de memória no Windows Server 2012
Habilitando a combinação de página pode reduzir o uso de memória em servidores que têm muitas páginas privadas, pagináveis com conteúdos idênticos. Por exemplo, os servidores que executam várias instâncias do mesmo aplicativo intensivo de memória ou um único aplicativo que funciona com dados altamente repetitivos, podem ser bons candidatos para testar a combinação de página. A desvantagem de habilitar a combinação de página é o aumento no uso da CPU.

Aqui estão alguns exemplos de funções de servidor em que a combinação de página é improvável que oferecem muitos benefícios:

-   Servidores de arquivos (a maioria da memória é consumida pelas páginas do arquivo que não são particulares e, portanto, não podem ser combinadas)

-   Microsoft SQL Servers que são configurados para usar AWE ou páginas grandes (a maioria da memória é privado, mas não-paginável)

A combinação de página está desabilitada por padrão, mas pode ser habilitado usando o [Enable-MMAgent](https://technet.microsoft.com/library/jj658954.aspx) cmdlet do Windows PowerShell. A combinação de página foi adicionada no Windows Server 2012.
