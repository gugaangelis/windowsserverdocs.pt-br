---
title: Configurar sistemas operacionais convidados para backups baseados em VSS para habilitar instantâneos consistentes com o aplicativo para a réplica do Hyper-V
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: f7a77b2cb743f478525f839e1c64ecc892b3fb04
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862059"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configurar sistemas operacionais convidados para backups baseados em VSS para habilitar instantâneos consistentes com o aplicativo para a réplica do Hyper-V

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Error|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Os instantâneos consistentes com o aplicativo exigem que o VSS (serviços de cópias de sombra de volume) esteja habilitado e configurado nos sistemas operacionais convidados de máquinas virtuais que participam da replicação.*  
  
## <a name="impact"></a>Impacto  
*Mesmo que os instantâneos consistentes com o aplicativo sejam especificados na configuração de replicação, o Hyper-V não os usará, a menos que o VSS esteja configurado. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use a conexão de máquina virtual para instalar o Integration Services na máquina virtual.*  
  


