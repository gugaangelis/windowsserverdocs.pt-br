---
title: Habilitar todos os adaptadores de rede virtual configurados para uma máquina virtual
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 79971ff503afcc1a087aced579e30989233c2b4a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861959"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>Habilitar todos os adaptadores de rede virtual configurados para uma máquina virtual

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Um ou mais adaptadores de rede podem estar desabilitados em uma máquina virtual.*  
  
## <a name="impact"></a>Impacto  
  
*As seguintes máquinas virtuais podem não ter conectividade de rede:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Use Device Manager no sistema operacional convidado para habilitar todos os adaptadores de rede virtual. Se o adaptador não for necessário, use o Gerenciador do Hyper-V para removê-lo da máquina virtual.*  
  


