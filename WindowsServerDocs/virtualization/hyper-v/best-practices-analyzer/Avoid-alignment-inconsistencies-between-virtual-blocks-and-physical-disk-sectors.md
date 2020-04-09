---
title: Evite as inconsistências de alinhamento entre blocos virtuais e setores de disco físico em discos rígidos virtuais dinâmicos ou discos diferenciais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3f090f015f2179ba372e56d580477ef8d72d977f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857799"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Evite as inconsistências de alinhamento entre blocos virtuais e setores de disco físico em discos rígidos virtuais dinâmicos ou discos diferenciais

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
*Foram detectadas inconsistências de alinhamento para um ou mais discos rígidos virtuais.*  
  
### <a name="impact"></a>Impacto  
*Se os discos rígidos virtuais estiverem armazenados no disco físico que tem um tamanho de setor de 4K, a máquina virtual ou os aplicativos que usam o disco rígido virtual poderão apresentar problemas de desempenho. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o assistente para criar disco rígido virtual para criar um novo disco rígido virtual em formato VHD ou VHDX e especifique o disco rígido virtual existente como o disco de origem. O novo disco rígido virtual será criado com o alinhamento entre os blocos virtuais e o disco físico.*  
  


