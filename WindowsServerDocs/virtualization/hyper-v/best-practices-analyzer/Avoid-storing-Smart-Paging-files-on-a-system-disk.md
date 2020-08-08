---
title: Evite armazenar arquivos de paginação inteligente em um disco do sistema
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 5ac7f8c9f6fb2edf2f6550cede9adfdc5be321f6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946069"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Evite armazenar arquivos de paginação inteligente em um disco do sistema

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Operações|

Nas seções a seguir, os itálicos indicam o texto que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*A configuração de memória para uma ou mais máquinas virtuais pode exigir o uso de paginação inteligente se a máquina virtual for reinicializada e o local especificado para o arquivo de paginação inteligente é o disco do sistema do servidor que executa o Hyper-V.*

## <a name="impact"></a>Impacto
*O uso do disco do sistema para paginação inteligente pode fazer com que o servidor que executa o Hyper-V tenha problemas. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Reconfigure as máquinas virtuais para armazenar os arquivos de paginação inteligente em um disco que não seja do sistema.*



