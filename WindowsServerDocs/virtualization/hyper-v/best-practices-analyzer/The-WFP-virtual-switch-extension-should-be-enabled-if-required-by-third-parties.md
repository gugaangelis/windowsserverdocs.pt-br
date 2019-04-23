---
title: a extensão do comutador virtual WFP deve ser habilitada se for exigida pelas extensões de terceiros
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5afe706c246276597b32400109370ba3129e5a24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850617"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>a extensão do comutador virtual WFP deve ser habilitada se for exigida pelas extensões de terceiros

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*A extensão de comutador virtual do Windows WFP (plataforma) está desabilitada.*  
  
## <a name="impact"></a>**Impacto**  
*Algumas extensões de comutador virtual de terceiros podem não funcionar corretamente os comutadores virtuais a seguir:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Use o cmdlet do Windows PowerShell, Enable-VMSwitchExtension, para habilitar a plataforma para filtros do Windows, se for exigida pelas extensões de terceiros.*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>Habilitar a plataforma para filtros do Windows usando o Windows PowerShell  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **inicie** e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com botão direito **Windows PowerShell** e clique em **executar como administrador**.  
  
3.  Execute este comando depois de substituir externo com o nome de seu comutador externo:  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name "Microsoft Windows Filtering Platform"  
```  
  
## <a name="see-also"></a>Consulte também  
[Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


