---
title: Verifique se o driver de função virtual opera corretamente quando uma máquina virtual está configurada para usar SR-IOV
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5e2c666973aa1ac0d5eb2c4e0d5d29793dc0ce75
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393620"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>Verifique se o driver de função virtual opera corretamente quando uma máquina virtual está configurada para usar SR-IOV

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
*O driver de função virtual não está operando corretamente no sistema operacional convidado de uma ou mais máquinas virtuais.*  
  
## <a name="impact"></a>Impacto  
*O desempenho de rede não é o ideal nas seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*In o sistema operacional convidado, faça o seguinte: Verifique se os drivers apropriados estão instalados e se todos os dispositivos de rede estão habilitados e verifique se há erros ou avisos no log de eventos.*  
  


