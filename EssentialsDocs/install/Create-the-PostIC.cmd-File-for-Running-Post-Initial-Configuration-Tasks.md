---
title: "Criar o arquivo PostIC.cmd para executar tarefas de configuração inicial do Post"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f5042204cd189e3101f5e0126fd98e786a49032d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>Criar o arquivo PostIC.cmd para executar tarefas de configuração inicial do Post

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar personalizações posteriores à configuração inicial escrevendo seu próprio código, e, em seguida, chamando o código de um arquivo de script chamado PostIC.cmd. Ao usar o arquivo PostIC.cmd, você deve seguir as diretrizes a seguir:  
  
-   Seu código de personalização deve ser executado silenciosamente (ele não pode exibir uma Interface do usuário).  
  
-   Seu código de personalização não pode iniciar uma reinicialização do servidor. A configuração inicial reiniciará o servidor como a última tarefa.  
  
-   Seu código de personalização deve ser executado em três minutos ou menos.  
  
 Defina seu arquivo PostIC.cmd para retornar 0 se o código é executado com êxito. Se qualquer outro valor é retornado, o sistema operacional procurará um arquivo chamado [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure), que contém o código que deve ser executado se o código no arquivo PostIC.cmd não for executado com êxito. Arquivo PostIC.cmd e o arquivo SetupFailure.cmd devem ser C:\Windows\Setup\Scripts localizado.  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>Para definir personalizações posteriores à configuração inicial  
  
1.  Escreva o código que é chamado do script PostIC.cmd.  
  
2.  Usando o bloco de notas, crie um arquivo chamado PostIC.cmd e adicione a chamada para o código que você criou na etapa 1. Certifique-se de que seu código retorna um valor de êxito.  
  
3.  Salve PostIC.cmd em C:\Windows\Setup\Scripts.  
  
4.  (Opcional) Crie um arquivo SetupFailure.cmd que executa o código se PostIC.cmd retornar algo diferente de 0.  
  
###  <a name="BKMK_SetupFailure"></a>SetupFailure.cmd  
 Você pode fornecer notificação de problemas na configuração inicial usando SetupFailure.cmd. O arquivo SetupFailure.cmd contém o código que você deseja executar caso ocorram problemas. O arquivo SetupFailure.cmd é colocado na C:\Windows\Setup\Scripts e é executado quando ocorre um problema com uma tarefa de configuração ou quando o arquivo PostIC.cmd retorna um valor diferente de 0.  
  
##### <a name="to-define-notifications"></a>Para definir notificações  
  
1.  Escreva o código que é chamado do script SetupFailure.cmd.  
  
2.  Usando o bloco de notas, crie um arquivo chamado SetupFailure.cmd e adicione a chamada para o código que você criou na etapa 1. Certifique-se de que seu código retorna um valor de êxito.  
  
3.  Salve SetupFailure.cmd em C:\Windows\Setup\Scripts.  
  
## <a name="see-also"></a>Consulte também  
 [Introdução ao Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)