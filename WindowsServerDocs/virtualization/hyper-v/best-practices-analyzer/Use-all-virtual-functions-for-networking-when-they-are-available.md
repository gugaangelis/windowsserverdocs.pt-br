---
title: Usar todas as funções virtuais para rede quando elas estiverem disponíveis
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0fab06ae21a4632df73b7a4d8b17b12665ffed98
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854209"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Usar todas as funções virtuais para rede quando elas estiverem disponíveis

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Alguns recursos de aceleração de hardware não estão sendo utilizados*  
  
## <a name="impact"></a>Impacto  
*Essa configuração pode fazer com que a utilização geral da CPU seja maior do que o necessário. O desempenho de rede pode não ser o ideal nas seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Considere configurar o adaptador de rede virtual para SR-IOV se o hardware físico oferecer suporte a SR-IOV e se essa configuração não entrar em conflito com os recursos de rede exigidos pela máquina virtual.*  
  


