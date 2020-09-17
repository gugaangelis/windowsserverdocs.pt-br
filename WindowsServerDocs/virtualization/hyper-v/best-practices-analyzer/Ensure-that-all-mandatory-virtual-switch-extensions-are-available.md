---
title: Verifique se todas as extensões obrigatórias do comutador virtual estão disponíveis
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
ms.date: 8/16/2016
ms.openlocfilehash: 8eeadf6ed8b90ef217d694e2f14b38314b8be7a5
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746851"
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



