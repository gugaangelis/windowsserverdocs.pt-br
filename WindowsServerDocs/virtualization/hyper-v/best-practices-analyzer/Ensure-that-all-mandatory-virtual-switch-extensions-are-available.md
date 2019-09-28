---
title: Verifique se todas as extensões obrigatórias do comutador virtual estão disponíveis
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c9363fbce35552a8f7d279662ae9072bcd7ea480
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364817"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Verifique se todas as extensões obrigatórias do comutador virtual estão disponíveis

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Um ou mais adaptadores de rede virtual estão conectados a um comutador virtual com extensões obrigatórias que estão desabilitadas ou não instaladas.*  
  
## <a name="impact"></a>Impacto  
*O tráfego de rede é bloqueado em um ou mais adaptadores de rede virtual nas seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*First, verifique se a extensão obrigatória foi instalada no host e instale a extensão, se necessário. Em seguida, se a extensão obrigatória estiver desabilitada, use o Gerenciador de comutador virtual ou o cmdlet do Windows PowerShell Enable-VMSwitchExtension para habilitar a extensão.*  
  


