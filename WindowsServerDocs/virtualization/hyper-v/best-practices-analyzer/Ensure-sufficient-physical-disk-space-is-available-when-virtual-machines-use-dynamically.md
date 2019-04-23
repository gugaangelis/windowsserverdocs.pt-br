---
title: Certifique-se de espaço suficiente em disco físico está disponível quando as máquinas virtuais usam discos rígidos virtuais de expansão dinâmica
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 09e481b99594ac543dadab2b60bf9b3f4c29e54b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883287"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Certifique-se de espaço suficiente em disco físico está disponível quando as máquinas virtuais usam discos rígidos virtuais de expansão dinâmica

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
*Uma ou mais máquinas virtuais estão usando discos rígidos virtuais de expansão dinâmica.*  
  
## <a name="impact"></a>Impacto  
*Expandir dinamicamente discos rígidos virtuais exigem espaço disponível no volume do host, de forma que pode ser alocado espaço quando ocorrem gravações em discos rígidos virtuais. Se o espaço disponível está esgotado, qualquer máquina virtual que se baseia no armazenamento físico pode ser afetada. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Monitorar espaço em disco disponível para garantir espaço suficiente está disponível para expansão. Considere desligar a máquina virtual e use o Assistente para edição de disco no Gerenciador do Hyper-V para converter cada disco rígido expansível dinamicamente para esta máquina virtual em um disco de rígido de virtual de tamanho fixo.*  
  


