---
title: Um ou mais adaptadores de rede devem ser configurados como o destino para o espelhamento de porta
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b83c166d-f010-47c4-a4bb-02167f2e3361
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: af51b854659adae1bf3132eed4d68e95467bdf85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364806"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-destination-for-port-mirroring"></a>Um ou mais adaptadores de rede devem ser configurados como o destino para o espelhamento de porta

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Uma ou mais máquinas virtuais têm um adaptador de rede configurado como uma origem para o espelhamento de porta, mas não há nenhum destino correspondente no comutador virtual.*  
  
## <a name="impact"></a>**Causa**  
*O espelhamento de porta não funcionará corretamente para os seguintes comutadores virtuais e máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Use o Windows PowerShell ou o Gerenciador do Hyper-V para concluir ou corrigir a configuração de espelhamento de porta.*  
  


