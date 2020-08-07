---
title: Verifique se todas as extensões obrigatórias do comutador virtual estão disponíveis
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 48c8ee80a0044c067d29730c1ec79bd67fdc062b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950250"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Verifique se todas as extensões obrigatórias do comutador virtual estão disponíveis

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*Um ou mais adaptadores de rede virtual estão conectados a um comutador virtual com extensões obrigatórias que estão desabilitadas ou não instaladas.*

## <a name="impact"></a>Impacto
*O tráfego de rede é bloqueado em um ou mais adaptadores de rede virtual nas seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Primeiro, verifique se a extensão obrigatória foi instalada no host e instale a extensão, se necessário. Em seguida, se a extensão obrigatória estiver desabilitada, use o Gerenciador de comutador virtual ou o cmdlet do Windows PowerShell Enable-VMSwitchExtension para habilitar a extensão.*



