---
title: Criar o Arquivo PostIC.cmd para Execução de Tarefas após a Configuração Inicial
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f23acf905e1c0b090076efd75d2e104a1cb0d186
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181352"
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

## <a name="see-also"></a>Consulte Também
 [Introdução com o Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)