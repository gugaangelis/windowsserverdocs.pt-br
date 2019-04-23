---
title: Evite usar discos rígidos virtuais com um tamanho de setor menor do que o tamanho de setor do armazenamento físico que armazena o arquivo de disco rígido virtual
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b6ec2e0995180ecf9ae5986447fdd460a1c7a337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846257"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Evite usar discos rígidos virtuais com um tamanho de setor menor do que o tamanho de setor do armazenamento físico que armazena o arquivo de disco rígido virtual

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Operando** <br />**Sistema**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Um ou mais discos rígidos virtuais têm um tamanho de setor físico for menor do que o tamanho de setor físico de armazenamento no qual o arquivo de disco rígido virtual está localizado.*  
  
## <a name="impact"></a>**Impacto**  
*Podem ocorrer problemas de desempenho na máquina virtual ou aplicativo que usa o disco rígido virtual. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Siga um destes procedimentos:*  
  
-   *Executar uma migração de armazenamento para mover o disco rígido virtual para um novo sistema físico*  
  
-   *Usar o Windows PowerShell ou o WMI para habilitar um disco rígido virtual do formato VHDX para relatar um tamanho de setor específico*  
  
-   *Usar uma configuração do registro para habilitar um disco rígido virtual do formato de VHD relatar um tamanho de setor físico de 4K*  
  


