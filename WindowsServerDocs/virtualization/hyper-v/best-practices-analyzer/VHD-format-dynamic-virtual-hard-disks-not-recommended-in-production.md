---
title: Discos rígidos virtuais dinâmicos de formato VHD não são recomendados para máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 324a60a0-1d15-4ef2-9f17-23cbd2eb42ce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bf5464208fc145a31571f01822bb5ba54efe89c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364594"
---
# <a name="vhd-format-dynamic-virtual-hard-disks-are-not-recommended-for-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Discos rígidos virtuais dinâmicos de formato VHD não são recomendados para máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção

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
*Uma ou mais máquinas virtuais usam discos rígidos virtuais de expansão dinâmica em formato VHD.*  
  
## <a name="impact"></a>**Causa**  
os discos rígidos virtuais dinâmicos de *VHD podem enfrentar problemas de consistência se ocorrer uma falha de energia. Problemas de consistência podem ocorrer se o disco físico executar uma atualização incompleta ou incorreta para um setor em um arquivo. vhd que está sendo modificado quando ocorrer uma falha de energia. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Shut a máquina virtual e converta o disco rígido virtual dinâmico do formato VHD em um disco rígido virtual em formato VHDX ou em um disco rígido virtual fixo. (O formato VHDX tem mecanismos de confiabilidade que ajudam a proteger o disco contra corrupções devido a falhas de energia do sistema.) No entanto, não converta o disco rígido virtual se for provável que ele esteja anexado a uma versão anterior do Windows em algum momento. As versões do Windows anteriores ao Windows Server 2012 não dão suporte ao formato VHDX.*  
  


