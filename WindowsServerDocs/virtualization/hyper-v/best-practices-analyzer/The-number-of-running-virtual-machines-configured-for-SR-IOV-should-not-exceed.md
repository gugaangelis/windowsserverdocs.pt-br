---
title: O número de máquinas virtuais configuradas para SR-IOV não deve exceder o número de funções virtuais disponíveis para as máquinas virtuais
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e63df9283927437f9cfc62c052d83b07fe599b34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829747"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>O número de máquinas virtuais configuradas para SR-IOV não deve exceder o número de funções virtuais disponíveis para as máquinas virtuais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Não há funções virtuais suficiente disponível para o número de máquinas virtuais configuradas para virtualização de e/s de raiz única (SR-IOV) em execução.*  
  
## <a name="impact"></a>Impacto  
*Desempenho de rede pode não ser ideal nas seguintes máquinas virtuais:*  
   
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Considere desabilitar a SR-IOV em um ou mais máquinas virtuais que não exigem uma função virtual SR-IOV.*  
  


