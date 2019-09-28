---
title: Configurar sistemas operacionais convidados para backups baseados em VSS para habilitar instantâneos consistentes com o aplicativo para a réplica do Hyper-V
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 032ca585da1c556fff6f9e06b3bde0662a5d64db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364965"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configurar sistemas operacionais convidados para backups baseados em VSS para habilitar instantâneos consistentes com o aplicativo para a réplica do Hyper-V

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Erro|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Os instantâneos consistentes com o aplicativo exigem que o VSS (serviços de cópias de sombra de volume) esteja habilitado e configurado nos sistemas operacionais convidados de máquinas virtuais que participam da replicação.*  
  
## <a name="impact"></a>Impacto  
*Even se os instantâneos consistentes com o aplicativo forem especificados na configuração de replicação, o Hyper-V não os usará, a menos que o VSS esteja configurado. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use a conexão de máquina virtual para instalar o Integration Services na máquina virtual.*  
  


