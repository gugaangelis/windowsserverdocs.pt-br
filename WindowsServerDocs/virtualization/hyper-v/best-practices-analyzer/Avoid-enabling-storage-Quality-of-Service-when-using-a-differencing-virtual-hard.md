---
title: Evite habilitar a qualidade de serviço de armazenamento ao usar um disco rígido virtual diferencial quando os discos rígidos virtuais pai e filho estiverem em volumes diferentes
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 4190b164b473e54c7fcecd68ddf8f746cebcfdc9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857779"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Evite habilitar a qualidade de serviço de armazenamento ao usar um disco rígido virtual diferencial quando os discos rígidos virtuais pai e filho estiverem em volumes diferentes

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Um disco rígido virtual diferencial com os discos rígidos virtuais pai e filho em volumes diferentes tem a qualidade de serviço de armazenamento habilitada.*  
  
## <a name="impact"></a>**Causa**  
*Essa configuração pode resultar em um comportamento de qualidade de serviço de armazenamento inesperado para o disco rígido virtual diferencial, bem como outros discos rígidos virtuais nos volumes pai e filho. Isso afeta os seguintes discos rígidos virtuais:*  
  
\<lista de discos rígidos virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Desabilite a qualidade de serviço de armazenamento nos discos rígidos virtuais referenciados ou execute uma migração de armazenamento para mover o disco rígido virtual pai e filho para o mesmo volume.*  
  


