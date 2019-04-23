---
title: Evite usar pontos de verificação em uma máquina virtual que executa uma carga de trabalho do servidor em um ambiente de produção
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 166ef839a40452cc4156144e10e9c666e7ce3472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856147"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>Evite usar pontos de verificação em uma máquina virtual que executa uma carga de trabalho do servidor em um ambiente de produção

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre as práticas recomendadas e varreduras, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Operações|  

Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.

> [!NOTE]  
> No Windows Server 2012 R2, os instantâneos de máquinas virtuais foram renomeados para pontos de verificação de máquina virtual no Gerenciador do Hyper-V para coincidir com a terminologia usada no gerenciamento de máquina Virtual do System Center. Para obter detalhes, consulte [Checkpoints and Snapshots Overview](https://technet.microsoft.com/library/dn818483.aspx).  
  
## <a name="issue"></a>Problema  
  
*Uma máquina virtual com um ou mais pontos de verificação foi encontrada.*  
  
## <a name="impact"></a>Impacto  
  
*Espaço disponível pode executá-los no disco físico que armazena os arquivos de pontos de verificação. Se isso ocorrer, nenhuma operação de disco adicional pode ser executada no armazenamento físico. Qualquer máquina virtual que se baseia no armazenamento físico pode ser afetada.*  
  
Se o espaço em disco físico de execução se esgotar, qualquer máquina virtual em execução que tem pontos de verificação ou discos rígidos virtuais armazenados nesse disco pode ser pausada automaticamente. Gerenciador do Hyper-V mostra o status dessas máquinas virtuais como "pausa crítica".  
  
## <a name="resolution"></a>Resolução  
  
*Se a máquina virtual executa uma carga de trabalho do servidor em um ambiente de produção, coloque a máquina virtual offline e, em seguida, use o Gerenciador do Hyper-V para aplicar ou excluir os pontos de verificação. Para excluir os pontos de verificação, você deve desligar a máquina virtual para concluir o processo.*  
  
> [!NOTE]  
> Pontos de verificação de produção agora estão disponíveis como uma alternativa para pontos de verificação padrão. Para obter detalhes, consulte [escolha entre pontos de verificação padrão ou de produção](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  


