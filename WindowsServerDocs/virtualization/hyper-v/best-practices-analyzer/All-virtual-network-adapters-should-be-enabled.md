---
title: Todos os adaptadores de rede virtual devem ser habilitados
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0a769c3203f6c6946f01cd91b66fbec38af83bbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837127"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Todos os adaptadores de rede virtual devem ser habilitados

>Aplica-se a: Windows Server 2016


  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Um ou mais adaptadores de rede virtual associados a um adaptador de rede física são desabilitados no sistema operacional de gerenciamento.*  
  
## <a name="impact"></a>Impacto  
  
*A configuração deste servidor não é ideal.*  
  
Sistema operacional de gerenciamento não pode se conectar a uma rede física de (externa) usando um dos adaptadores de rede física neste computador porque ela está associada a um adaptador de rede virtual desabilitado.  
  
## <a name="resolution"></a>Resolução  
  
*Use configurações de Internet e rede para habilitar o adaptador de rede virtual. Ou, use o Gerenciador de comutador Virtual para reconfigurar o comutador virtual externo para que ele não é compartilhado com o sistema operacional de gerenciamento.*  
  


