---
title: O número de máquinas virtuais em execução configuradas para SR-IOV não deve exceder o número de funções virtuais disponíveis para as máquinas virtuais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0887035c84ebc4b7d93163533387f2f8ab20fb87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364607"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>O número de máquinas virtuais em execução configuradas para SR-IOV não deve exceder o número de funções virtuais disponíveis para as máquinas virtuais

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
*Não há funções virtuais suficientes disponíveis para o número de máquinas virtuais em execução configuradas para SR-IOV (virtualização de e/s de raiz única).*  
  
## <a name="impact"></a>Impacto  
*O desempenho de rede pode não ser o ideal nas seguintes máquinas virtuais:*  
   
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Considere desabilitar a SR-IOV em uma ou mais máquinas virtuais que não exigem uma função virtual de SR-IOV.*  
  


