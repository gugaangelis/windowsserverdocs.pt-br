---
title: Evite usar discos rígidos virtuais com um tamanho de setor menor que o tamanho de setor do armazenamento físico que armazena o arquivo de disco rígido virtual
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 944fdce68a2f0b8e9c122f5f9134f0e07de18bbd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366411"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Evite usar discos rígidos virtuais com um tamanho de setor menor que o tamanho de setor do armazenamento físico que armazena o arquivo de disco rígido virtual

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Operacional** <br />**Sistema**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Um ou mais discos rígidos virtuais têm um tamanho de setor físico menor do que o tamanho do setor físico do armazenamento no qual o arquivo do disco rígido virtual está localizado.*  
  
## <a name="impact"></a>**Causa**  
*Podem ocorrer problemas de desempenho na máquina virtual ou no aplicativo que usa o disco rígido virtual. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Siga um destes procedimentos:*  
  
-   *Executar uma migração de armazenamento para mover o disco rígido virtual para um novo sistema físico*  
  
-   *Use o Windows PowerShell ou o WMI para habilitar um disco rígido virtual de formato VHDX para relatar um tamanho de setor específico*  
  
-   *Use uma configuração do registro para habilitar um disco rígido virtual em formato VHD para relatar um tamanho de setor físico de 4K*  
  


