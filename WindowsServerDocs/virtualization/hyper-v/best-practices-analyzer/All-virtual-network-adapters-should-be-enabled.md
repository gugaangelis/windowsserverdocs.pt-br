---
title: Todos os adaptadores de rede virtual devem ser habilitados
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 450a3b42529be9a85991fcaf5263bae7b7827b1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857829"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Todos os adaptadores de rede virtual devem ser habilitados

>Aplica-se a: Windows Server 2016


  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Um ou mais adaptadores de rede virtual associados a um adaptador de rede física estão desabilitados no sistema operacional de gerenciamento.*  
  
## <a name="impact"></a>Impacto  
  
*A configuração deste servidor não é a ideal.*  
  
O sistema operacional de gerenciamento não pode se conectar a uma rede física (externa) usando um dos adaptadores de rede física neste computador porque ele está associado a um adaptador de rede virtual desabilitado.  
  
## <a name="resolution"></a>Resolução  
  
*Use as configurações de rede & Internet para habilitar o adaptador de rede virtual. Ou use o Gerenciador de comutador virtual para reconfigurar o comutador virtual externo para que ele não seja compartilhado com o sistema operacional de gerenciamento.*  
  


