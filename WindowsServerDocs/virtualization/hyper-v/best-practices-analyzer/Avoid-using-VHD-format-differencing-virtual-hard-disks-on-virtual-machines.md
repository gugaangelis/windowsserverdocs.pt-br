---
title: Evite usar o formato VHD de diferenciação discos rígidos virtuais em máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: da908d00a6b5c48a61dad89e8c7b08cf80b4314c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819177"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Evite usar o formato VHD de diferenciação discos rígidos virtuais em máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção

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
*Uma ou mais máquinas virtuais usam discos rígidos virtuais diferenciais em formato VHD.*  
  
## <a name="impact"></a>**Impacto**  
*Discos rígidos virtuais diferenciais em formato VHD pode apresentar problemas de consistência se ocorrer uma falha de energia. Problemas de consistência podem acontecer se o disco físico executa uma atualização incompleta ou incorreta para um setor em um arquivo. vhd que está sendo modificado quando ocorre uma falha de energia. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Desligue a máquina virtual e converter a cadeia de diferenciação discos rígidos virtuais para o formato VHDX em formato VHD ou mesclar a cadeia em um disco rígido virtual fixo. (O formato VHDX tem mecanismos de confiabilidade que ajudam a proteger o disco de corrompidas devido a falhas de energia). No entanto, não converta o disco rígido virtual se tiver probabilidade de ser anexado a uma versão anterior do Windows em algum momento. Versões do Windows anteriores ao Windows Server 2012 não dão suporte para o formato VHDX.*  
  


