---
title: Evitar ter que pausar uma máquina virtual
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4492ac385a289d075ebcd48b1c7c1c78c1af2f8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814347"
---
# <a name="avoid-pausing-a-virtual-machine"></a>Evitar ter que pausar uma máquina virtual

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
  
*Este servidor tem uma ou mais máquinas virtuais em um estado pausado.*  
  
## <a name="impact"></a>Impacto  
  
*Dependendo da quantidade de memória disponível, você pode não ser capaz de executar máquinas virtuais adicionais.*  
  
Máquinas de virtuais em pausa não liberar a memória alocada, o que significa que a memória não está disponível para iniciar outras máquinas virtuais.  
  
## <a name="resolution"></a>Resolução  
  
*Se isso é intencional, nenhuma ação adicional é necessária. Caso contrário, considere retomando essas máquinas virtuais ou desligá-los.*  
  
#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>Use o Gerenciador do Hyper-V para retomar a máquina virtual  
  
1.  Abra o Gerenciador Hyper-V. (Da **ferramentas** menu do Gerenciador do servidor, clique em **Gerenciador do Hyper-V**.)  
  
2.  Dos **máquinas virtuais** listar, localizar as máquinas virtuais com um estado de **pausado**.  
  
    > [!IMPORTANT]  
    > Um estado de **pausado-crítico** ocorre quando há muito pouco espaço livre restante no armazenamento físico para a máquina virtual. Antes de tentar retomar uma máquina virtual nesse estado, libere espaço disponível no armazenamento físico.  
  
3.  Cada nome de máquina virtual com o botão direito e, em seguida, clique em **retomar**. Isso retorna a máquina virtual ao estado de execução. Depois disso, se você quiser desligar a máquina virtual, clique com botão direito-lo novamente e escolha **desligar**.  
  
#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>Usar o Windows PowerShell para retomar a máquina virtual  
  
Você pode fazer isso em um comando usando a filtragem e o pipeline depois de obter todas as máquinas virtuais no host. Digite:  
  
```  
get-vm | where state -eq 'paused' | resume-vm  
```  
  


