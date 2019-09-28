---
title: a extensão do comutador virtual WFP deve ser habilitada se for exigida pelas extensões de terceiros
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 41ab2bac7c98608b051c74d2fbfb8359f493385c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364621"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>a extensão do comutador virtual WFP deve ser habilitada se for exigida pelas extensões de terceiros

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
*A extensão do comutador virtual da Windows Filtering Platform (WFP) está desabilitada.*  
  
## <a name="impact"></a>**Causa**  
*Algumas extensões de comutador virtual de terceiros podem não funcionar corretamente nos seguintes comutadores virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Use o cmdlet do Windows PowerShell, Enable-VMSwitchExtension, para habilitar a plataforma de filtragem do Windows se ela for exigida por extensões de terceiros.*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>Habilitar a plataforma de filtragem do Windows usando o Windows PowerShell  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **Iniciar** e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com o botão direito do mouse em **Windows PowerShell** e clique em **Executar como administrador**.  
  
3.  Execute este comando depois de substituir external pelo nome do seu comutador externo:  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name "Microsoft Windows Filtering Platform"  
```  
  
## <a name="see-also"></a>Consulte também  
[Habilitar-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


