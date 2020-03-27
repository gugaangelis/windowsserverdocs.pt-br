---
title: Criar o Arquivo PostIC.cmd para Execução de Tarefas após a Configuração Inicial
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 81a38f0baf3a47323f6bf8836e48d02bc955cde0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312060"
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>Criar o Arquivo PostIC.cmd para Execução de Tarefas após a Configuração Inicial

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar personalizações à configuração pós-inicial escrevendo seu próprio código e então chamando esse código de um arquivo de script chamado PostIC.cmd. Ao usar o arquivo PostIC.cmd, é preciso cumprir as seguintes diretrizes:  
  
- O código de personalização deve ser executado silenciosamente (não é possível exibir uma Interface de Usuário).  
  
- O código de personalização não pode iniciar uma reinicialização do servidor. A Configuração Inicial irá reiniciar o servidor como a última tarefa.  
  
- O código de personalização deve ser executado em três minutos ou menos.  
  
  Configure o arquivo PostIC.cmd para retornar um 0 se o código for executado com êxito. Se for retornado qualquer outro valor, o sistema operacional procurará um arquivo nomeado [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure), que contém um código que deverá ser executado caso o código do arquivo PostIC.cmd não seja executado com êxito. O arquivo PostIC.cmd e o arquivo SetupFailure.cmd devem estar localizados em C:\Windows\Setup\Scripts.  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>Para definir as personalizações após a configuração inicial  
  
1.  Escreva o código chamado a partir do script PostIC.cmd.  
  
2.  Usando o Bloco de Notas, crie um arquivo chamado PostIC.cmd e adicione a chamada ao código criado na etapa 1. Verifique se seu código retorna um valor de êxito.  
  
3.  Salve PostIC.cmd em C:\Windows\Setup\Scripts.  
  
4.  (Opcional) Crie um arquivo SetupFailure.cmd que executará o código se PostIC.cmd retornar algo diferente de 0.  
  
###  <a name="setupfailurecmd"></a><a name="BKMK_SetupFailure"></a>SetupFailure. cmd  
 Você pode fornecer notificação de problemas na Configuração Inicial usando SetupFailure.cmd. O arquivo SetupFailure.cmd contém o código que você deseja executar se ocorrerem problemas. O arquivo SetupFailure.cmd está em C:\Windows\Setup\Scripts e será executado em caso de problemas com uma tarefa de configuração ou quando o arquivo PostIC.cmd retornar um valor diferente de 0.  
  
##### <a name="to-define-notifications"></a>Para definir notificações  
  
1.  Escreva o código chamado a partir do script SetupFailure.cmd.  
  
2.  Usando o Bloco de Notas, crie um arquivo chamado SetupFailure.cmd e adicione a chamada ao código criado na etapa 1. Verifique se seu código retorna um valor de êxito.  
  
3.  Salve o SetupFailure.cmd em C:\Windows\Setup\Scripts.  
  
## <a name="see-also"></a>Consulte também  
 [Introdução com o Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)