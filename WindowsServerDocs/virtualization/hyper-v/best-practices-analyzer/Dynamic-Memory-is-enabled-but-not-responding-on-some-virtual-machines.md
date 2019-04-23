---
title: Memória dinâmica está habilitada, mas não está respondendo em algumas máquinas virtuais
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 95fd426929f3e2f6f01bc10b207a21a57f1d8370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887777"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>Memória dinâmica está habilitada, mas não está respondendo em algumas máquinas virtuais

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
*Uma ou mais máquinas virtuais está tendo problemas com o driver necessário para a memória dinâmica no sistema operacional convidado.*  
  
## <a name="impact"></a>Impacto  
*O sistema operacional nas seguintes máquinas virtuais pode não ser executado ou pode ser executado de modo não confiável porque o Hyper-V não é possível ajustar a memória dinamicamente para responder a alterações na demanda de memória. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Esse comportamento é esperado se a máquina virtual está sendo inicializado. Se a máquina virtual não está inicializando, certifique-se de que os serviços de integração são atualizados para a versão mais recente e o sistema operacional convidado dá suporte a memória dinâmica.*  
  
A partir do Windows Server 2016, os serviços de integração são entregues por meio do Windows Update. Certifique-se de que as máquinas virtuais são configuradas para receber atualizações para obter a versão mais recente do integration services.  
  
Memória dinâmica funciona com versões específicas de convidados com suporte. Ver [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx) para versões anteriores ao Windows Server 2016 e Windows 10.  
  


