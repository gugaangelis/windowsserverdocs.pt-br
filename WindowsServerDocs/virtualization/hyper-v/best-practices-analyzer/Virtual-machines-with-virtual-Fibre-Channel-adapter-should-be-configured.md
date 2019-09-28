---
title: Máquinas virtuais configuradas com um adaptador de Fibre Channel virtual devem ser configuradas para alta disponibilidade para o armazenamento baseado em Fibre Channel
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b4c50ac70b51ab6a2e5cb8247b309070d85932a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364564"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Máquinas virtuais configuradas com um adaptador de Fibre Channel virtual devem ser configuradas para alta disponibilidade para o armazenamento baseado em Fibre Channel

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Informações|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Uma ou mais máquinas virtuais não têm uma conexão altamente disponível com o armazenamento baseado em Fibre Channel porque essas máquinas virtuais são configuradas com um adaptador de Fibre Channel virtual que está conectado a apenas um HBA (adaptador de barramento de host).*  
  
## <a name="impact"></a>**Causa**  
a falha de @no__t 0A do adaptador de barramento de host pode bloquear a conexão de Fibre Channel entre o armazenamento e as máquinas virtuais. Isso afeta as seguintes máquinas virtuais: *  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Adicione outra conexão da máquina virtual ao adaptador de barramento de host e configure o MPIO (Multipath I/O) no sistema operacional convidado para estabelecer conexões de Fibre Channel redundantes.*  
  


