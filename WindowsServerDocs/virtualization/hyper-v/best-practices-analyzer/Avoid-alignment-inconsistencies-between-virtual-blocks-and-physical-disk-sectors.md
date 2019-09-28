---
title: Evite as inconsistências de alinhamento entre blocos virtuais e setores de disco físico em discos rígidos virtuais dinâmicos ou discos diferenciais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f77d80db1e2454eb460043cacef632e979fc16de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366487"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Evite as inconsistências de alinhamento entre blocos virtuais e setores de disco físico em discos rígidos virtuais dinâmicos ou discos diferenciais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Foram detectadas inconsistências de alinhamento para um ou mais discos rígidos virtuais.*  
  
### <a name="impact"></a>Impacto  
*If os discos rígidos virtuais são armazenados no disco físico que tem um tamanho de setor de 4K, a máquina virtual ou os aplicativos que usam o disco rígido virtual podem apresentar problemas de desempenho. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o assistente para criar disco rígido virtual para criar um novo disco rígido virtual de formato VHD ou de formato VHDX e especificar o disco rígido virtual existente como o disco de origem. O novo disco rígido virtual será criado com o alinhamento entre os blocos virtuais e o disco físico.*  
  


