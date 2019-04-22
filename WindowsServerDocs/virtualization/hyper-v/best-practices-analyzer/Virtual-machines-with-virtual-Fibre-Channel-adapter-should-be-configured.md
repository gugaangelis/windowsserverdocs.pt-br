---
title: Máquinas virtuais configuradas com um adaptador de Fibre Channel virtual deve ser configuradas para alta disponibilidade para o armazenamento baseado em Fibre Channel
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 203477a022f7c5f819ef7b99f1b8e37a0b8b0a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816937"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Máquinas virtuais configuradas com um adaptador de Fibre Channel virtual deve ser configuradas para alta disponibilidade para o armazenamento baseado em Fibre Channel

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Informações do|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Uma ou mais máquinas virtuais sem uma conexão altamente disponível para armazenamento baseado em Fibre Channel, porque essas máquinas virtuais são configuradas com um adaptador de Fibre Channel virtual que está conectado ao adaptador de barramento de apenas um host (HBA).*  
  
## <a name="impact"></a>**Impacto**  
*Uma falha do adaptador de barramento do host pode bloquear a conexão de Fibre Channel entre o armazenamento e as máquinas virtuais. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Adicione outra conexão da máquina virtual para o adaptador de barramento do host e configure o Multipath i / o (MPIO) no sistema operacional convidado para estabelecer conexões redundantes de Fibre Channel.*  
  


