---
title: Uma SAN virtual deve ser associada a um adaptador de barramento de host físico
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9e86f8d9b9a4a87fd6457954c3a4723857faac3b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366694"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Uma SAN virtual deve ser associada a um adaptador de barramento de host físico

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
*Uma SAN (rede de área de armazenamento) virtual foi configurada sem uma associação a um adaptador de barramento de host (HBA).*  
  
## <a name="impact"></a>**Causa**  
a máquina virtual *A não será iniciada quando estiver configurada com um adaptador de Fibre Channel virtual conectado a uma SAN virtual configurada incorretamente. Isso afeta as seguintes SANs virtuais:*  
  
  
\<list de SANs virtuais >  
  
  
## <a name="resolution"></a>**Resolução**  
*Reconfigure a rede SAN virtual conectando-a a um adaptador de barramento de host.*  
  
  
  


