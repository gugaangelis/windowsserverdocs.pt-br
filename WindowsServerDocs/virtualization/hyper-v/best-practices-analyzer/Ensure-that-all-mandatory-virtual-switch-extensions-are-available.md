---
title: Certifique-se de que todas as extensões de comutador virtual obrigatórios estão disponíveis
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 53ceeb9aab6ca7196454fbcd7f0fdae8b34d05d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825937"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Certifique-se de que todas as extensões de comutador virtual obrigatórios estão disponíveis

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
*Um ou mais adaptadores de rede virtual estão conectados a um comutador virtual com extensões obrigatórios que estão desativados ou não instalado.*  
  
## <a name="impact"></a>Impacto  
*O tráfego de rede é bloqueado em um ou mais adaptadores de rede virtual nas seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Primeiro, certifique-se de que a extensão obrigatória foi instalado no host e instalar a extensão, se necessário. Em seguida, se a extensão obrigatória for desabilitado, use o Gerenciador de comutador Virtual ou o cmdlet Enable-VMSwitchExtension do Windows PowerShell para habilitar a extensão.*  
  


