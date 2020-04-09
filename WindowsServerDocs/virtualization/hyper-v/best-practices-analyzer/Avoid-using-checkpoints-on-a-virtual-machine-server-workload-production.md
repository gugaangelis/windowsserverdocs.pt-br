---
title: Evite usar pontos de verificação em uma máquina virtual que executa uma carga de trabalho de servidor em um ambiente de produção
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: f0e9d40fa6e28b515621402b853012cb59086a07
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857709"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>Evite usar pontos de verificação em uma máquina virtual que executa uma carga de trabalho de servidor em um ambiente de produção

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Operações|  

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

> [!NOTE]  
> No Windows Server 2012 R2, os instantâneos de máquina virtual foram renomeados para pontos de verificação de máquina virtual no Gerenciador do Hyper-V para corresponder à terminologia usada no gerenciamento de máquinas virtuais do System Center. Para obter detalhes, consulte [visão geral de pontos de verificação e instantâneos](https://technet.microsoft.com/library/dn818483.aspx).  
  
## <a name="issue"></a>Problema  
  
*Uma máquina virtual com um ou mais pontos de verificação foi encontrada.*  
  
## <a name="impact"></a>Impacto  
  
*O espaço disponível pode ficar no disco físico que armazena os arquivos de pontos de verificação. Se isso ocorrer, nenhuma operação de disco adicional poderá ser executada no armazenamento físico. Qualquer máquina virtual que dependa do armazenamento físico poderá ser afetada.*  
  
Se o espaço em disco físico for esgotado, qualquer máquina virtual em execução que tenha pontos de verificação ou discos rígidos virtuais armazenados nesse disco poderá ser pausada automaticamente. O Gerenciador do Hyper-V mostra o status dessas máquinas virtuais como em pausa-crítico.  
  
## <a name="resolution"></a>Resolução  
  
*Se a máquina virtual executar uma carga de trabalho de servidor em um ambiente de produção, coloque a máquina virtual offline e, em seguida, use o Gerenciador do Hyper-V para aplicar ou excluir os pontos de verificação. Para excluir pontos de verificação, você deve desligar a máquina virtual para concluir o processo.*  
  
> [!NOTE]  
> Os pontos de verificação de produção agora estão disponíveis como uma alternativa aos pontos de verificação padrão. Para obter detalhes, consulte [escolher entre pontos de verificação padrão ou de produção](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  


