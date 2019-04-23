---
title: Serviços de integração devem ser instalados antes do primário ou máquinas virtuais de réplica pode usar um endereço IP alternativo após um failover
description: Versão online do texto para essa regra do analisador de práticas recomendadas, com links para obter mais informações.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 1ff8dbfd71655aee86ba7d0feac87ec2267a2171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865507"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Serviços de integração devem ser instalados antes do primário ou máquinas virtuais de réplica pode usar um endereço IP alternativo após um failover

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*As máquinas virtuais que participam da replicação pode ser configurado para usar um endereço IP específico no caso de failover, mas somente se os serviços de integração estão instalados no sistema operacional convidado da máquina virtual.*  
  
## <a name="impact"></a>Impacto  
*No caso de um failover (planejado, não planejado ou de teste), a máquina virtual de réplica virão online usando o mesmo endereço IP da máquina virtual primária. Essa configuração poderá causar problemas de conectividade. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use a Conexão de máquina Virtual para instalar o integration services na máquina virtual.*  
  
A partir do Windows Server 2016, os serviços de integração para máquinas de virtuais do Windows são entregues por meio do Windows Update. Certifique-se de que essas máquinas virtuais são configuradas para receber atualizações do Windows para obter a versão mais recente do integration services. O kernel do Linux agora inclui serviços de integração do Linux (LIS) e é atualizado para novas versões, mas distribuições do Linux com base nos kernels mais antigos não podem ter as mais recentes aprimoramentos ou correções. Para obter detalhes, consulte [máquinas virtuais de suporte para Linux e FreeBSD para Hyper-V no Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


