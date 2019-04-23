---
title: Hyper-V deve ser a única função habilitada
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bd03554396696a43b4821aff0f4ed893933484c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886457"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-V deve ser a única função habilitada

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Funções que não sejam do Hyper-V estão habilitadas neste servidor.*  
  
Na maioria dos casos, não é uma boa ideia para instalar outras funções em um servidor que executa a função Hyper-V. Serviço de função Host de virtualização de área de trabalho remota é uma exceção, porque ele faz parte da função Serviços de área de trabalho remota e requer o Hyper-V para ser instalado no mesmo servidor.  
  
## <a name="impact"></a>Impacto  
  
*A função Hyper-V deve ser a única função habilitada em um servidor.*  
  
Essa prática ajuda a manter o sistema operacional do host livre de funções, recursos e aplicativos que não são necessários para executar o Hyper-V. Seguir essa recomendação e executando o Hyper-V no Nano Server ajuda a reduzir o número de atualizações, será necessário porque somente o Nano Server, os componentes de serviço do Hyper-V e o hipervisor do Windows seria sujeito a atualizações de software.  
  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador do servidor para remover todas as funções, exceto o Hyper-V.*  
  
Gerenciador do servidor inclui o Assistente para remover funções. Este assistente permite remover mais de uma função ao mesmo tempo. Antes de remover funções, o Assistente para remover funções verifica as dependências reduzir o risco de remover o software que dependem de outras funções. Se as dependências forem encontradas, o assistente solicita que você aprove a remoção de outras funções, serviços de função ou de software necessários para as funções instaladas.   
  
Para usar o Gerenciador do servidor, você deve fazer logon no computador como administrador.  
  
#### <a name="to-remove-a-role"></a>Para remover uma função  
  
1.  Abra o Gerenciador do servidor usando atalhos na **iniciar** menu, na barra de tarefas do Windows ou em Ferramentas administrativas.  
2.   No **resumo de funções** área da janela principal do Gerenciador do servidor, clique em **remover funções**. Siga as instruções no Assistente para remover a função.   
  
  
  


