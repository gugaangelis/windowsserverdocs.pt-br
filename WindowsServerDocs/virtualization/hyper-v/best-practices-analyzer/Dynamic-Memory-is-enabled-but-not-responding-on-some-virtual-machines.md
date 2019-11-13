---
title: Memória Dinâmica está habilitado, mas não respondendo em algumas máquinas virtuais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9aa482d91c94a7a619bb65046cf152d6a5f8827a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393682"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>Memória Dinâmica está habilitado, mas não respondendo em algumas máquinas virtuais

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
*Uma ou mais máquinas virtuais estão enfrentando problemas com o driver necessário para Memória Dinâmica no sistema operacional convidado.*  
  
## <a name="impact"></a>Impacto  
*O sistema operacional convidado nas seguintes máquinas virtuais pode não ser executado ou pode ser executado de forma não confiável porque o Hyper-V não pode ajustar a memória dinamicamente para responder às alterações na demanda de memória. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Esse comportamento será esperado se a máquina virtual estiver inicializando. Se a máquina virtual não estiver inicializando, verifique se os serviços de integração estão atualizados para a versão mais recente e se o sistema operacional convidado dá suporte a Memória Dinâmica.*  
  
A partir do Windows Server 2016, o Integration Services é fornecido por meio de Windows Update. Verifique se as máquinas virtuais estão configuradas para receber atualizações para obter a versão mais recente do Integration Services.  
  
Memória Dinâmica funciona com versões específicas de convidados com suporte. Consulte [visão geral de memória dinâmica do Hyper-V](https://technet.microsoft.com/library/hh831766.aspx) para versões anteriores ao windows Server 2016 e Windows 10.  
  


