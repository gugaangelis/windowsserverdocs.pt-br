---
title: Uma SAN virtual deve ser associada a um adaptador de barramento de host físico
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 10a17f8661f1436f5b4db317648edde57a0dfe74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857929"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Uma SAN virtual deve ser associada a um adaptador de barramento de host físico

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
*Uma SAN (rede de área de armazenamento) virtual foi configurada sem uma associação a um adaptador de barramento de host (HBA).*  
  
## <a name="impact"></a>**Causa**  
*Uma máquina virtual não será iniciada quando estiver configurada com um adaptador de Fibre Channel virtual conectado a uma SAN virtual configurada incorretamente. Isso afeta as seguintes SANs virtuais:*  
  
  
\<lista de SANs virtuais >  
  
  
## <a name="resolution"></a>**Resolução**  
*Reconfigure a rede SAN virtual conectando-a a um adaptador de barramento de host.*  
  
  
  


