---
title: Configurar máquinas virtuais para usar o SR-IOV somente quando houver suporte do sistema operacional convidado
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8c43e06806f66ce0faae255f0f34d80a653fbe10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366261"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>Configurar máquinas virtuais para usar o SR-IOV somente quando houver suporte do sistema operacional convidado

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
*Uma ou mais máquinas virtuais estão configuradas para usar O SR-IOV (virtualização de e/s de raiz única), mas o sistema operacional convidado não oferece suporte a SR-IOV*  
  
## <a name="impact"></a>Impacto  
*As funções virtuais SR-IOV não serão alocadas para as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Desabilite o SR-IOV em todas as máquinas virtuais que executam sistemas operacionais convidados que não dão suporte a SR-IOV.*  
  
O SR-IOV tem suporte apenas em alguns convidados do Windows de 64 bits. Para obter detalhes, consulte [compatibilidade de recursos do Hyper-V por geração e convidado](../Hyper-V-feature-compatibility-by-generation-and-guest.md).  
  


