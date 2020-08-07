---
title: A ressincronização da replicação deve ser agendada para o horário de pico
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: dad983e19df328946f3bd7ec59f68ee3e9f8bfcc
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939002"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>A ressincronização da replicação deve ser agendada para o horário de pico

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Operações|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*A ressincronização da replicação para as máquinas virtuais primárias não está agendada para horários de pico.*

## <a name="impact"></a>Impacto
*Quanto mais longa uma máquina virtual estiver em um estado que exija ressincronização, mais tempo os arquivos de log de replicação aumentarão e mais alterações não replicadas ocorrerão nas máquinas virtuais primárias. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Use o Gerenciador do Hyper-V para modificar as configurações de replicação da máquina virtual para executar a ressincronização automaticamente fora do horário de pico.*



