---
title: Garantir que o espaço em disco físico suficiente esteja disponível quando as máquinas virtuais usam discos rígidos virtuais de expansão dinâmica
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c38d7c11a05eef9d29097e625fec2830000cf550
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393636"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Garantir que o espaço em disco físico suficiente esteja disponível quando as máquinas virtuais usam discos rígidos virtuais de expansão dinâmica

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
*Uma ou mais máquinas virtuais estão usando discos rígidos virtuais de expansão dinâmica.*  
  
## <a name="impact"></a>Impacto  
*A expansão dinâmica de discos rígidos virtuais requer espaço disponível no volume de hospedagem para que o espaço possa ser alocado quando as gravações nos discos rígidos virtuais ocorrerem. Se o espaço disponível for esgotado, qualquer máquina virtual que dependa do armazenamento físico poderá ser afetada. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Monitore o espaço em disco disponível para garantir que haja espaço suficiente disponível para expansão. Considere desligar a máquina virtual e usar o assistente para editar disco no Gerenciador do Hyper-V para converter cada disco rígido virtual de expansão dinâmica dessa máquina virtual em um disco rígido virtual de tamanho fixo.*  
  


