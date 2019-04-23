---
title: shutdown
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d03a8d35f3e56ec7829bc51c1499fddd13b86e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837247"
---
# <a name="shutdown"></a>shutdown



Permite desligar ou reiniciar computadores locais ou remotos um por vez.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c "comment"]] 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/i|Exibe a **caixa de diálogo de desligamento remoto** caixa. O **/i** opção deve ser o primeiro parâmetro, o comando a seguir. Se **/i** for especificado, todas as outras opções são ignoradas.|
|/l|Faz logoff do usuário atual imediatamente, com nenhum período de tempo limite. Não é possível usar **/l** com **/m** ou **/t**.|
|/s|Desliga o computador.|
|/r|Reinicia o computador após o desligamento.|
|/a|Anula um desligamento do sistema. Em vigor somente durante o período de tempo limite. Para usar **/a**, você também deve usar o **/m** opção.|
|/p|Desliga o computador local somente (não um computador remoto) — sem período de tempo limite ou aviso. Você pode usar **/p** somente com **/d** ou **/f**. Se seu computador não oferece suporte a funcionalidade de desligar, ele será desligado quando você usa **/p**, mas a capacidade do computador permanecerá no disco.|
|/h|Coloca o computador local em hibernação, se a hibernação estiver ativada. Você pode usar **/h** somente com **/f**.|
|/e|Permite documentar o motivo do desligamento inesperado no computador de destino.|
|/f|Força a execução de aplicativos para fechar sem avisar os usuários.</br>Cuidado: Usando o **/f** opção pode resultar em perda de dados não salvos.|
|/m \\\\\<ComputerName>|Especifica o computador de destino. Não pode ser usado com o **/l** opção.|
|/t \<XXX>|Define o período de tempo limite ou atraso nas *XXX* segundos antes de uma reinicialização ou desligamento. Isso faz com que um aviso exibir no console local. Você pode especificar 0 a 600 segundos. Se você não usar **/t**, o período de tempo limite é de 30 segundos por padrão.|
|/d [p\|u:]\<XX>:\<YY>|Lista o motivo da reinicialização do sistema ou do desligamento. A seguir está os valores de parâmetro:</br>**p** indica que a reinicialização ou desligamento é planejado.</br>**u** indica que o motivo é definido pelo usuário.</br>Observação: Se **p** ou **u** não forem especificadas, a reinicialização ou desligamento é planejado.</br>*XX* Especifica o número de motivo principal (inteiro positivo menor que 256).</br>*AA* Especifica o número de motivo secundário (inteiro positivo menor que 65536).|
|/c "\<Comment>"|Permite que você explique detalhadamente a razão do desligamento. Primeiro você deve fornecer um motivo usando o **/d** opção. Você deve colocar comentários entre aspas. É possível usar até 511 caracteres.|
|/?|Exibe a Ajuda no prompt de comando, incluindo uma lista dos motivos principais e secundárias que são definidos em seu computador local.|

## <a name="remarks"></a>Comentários

-   Os usuários devem ser atribuídos a **desligar o sistema** administrados de direito de usuário para desligados local ou remotamente no computador que está usando o **desligamento** comando.
-   Os usuários devem ser membros do grupo Administradores para anotar um desligamento inesperado de um computador local ou administrado remotamente. Se o computador de destino tiver ingressado em um domínio, os membros do grupo Admins. do domínio podem ser capazes de executar esse procedimento. Para obter mais informações, consulte:  
    -   [Grupos locais padrão](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [Grupos padrão](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   Se você quiser desligar mais de um computador por vez, você pode chamar **desligamento** para cada computador usando um script, ou você pode usar **desligamento** **/i** para exibir o controle remoto Caixa de diálogo de desligamento.
-   Se você especificar códigos de motivo principal e secundária, você deve primeiro definir esses códigos de motivo em cada computador em que você planeja usar os motivos. Se os códigos de motivo não forem definidos no computador de destino, os eventos de desligamento não é possível registrar o texto de razão correto.
-   Lembre-se indicar que um desligamento é planejado usando o **p:** parâmetro. A omissão **p:** indica que um desligamento é planejado. Se você digitar **p:** seguido do código de motivo para um desligamento planejado, o comando não realizará o desligamento. Por outro lado, se você omitir **p:** e o tipo no código de motivo para um desligamento planejado, o comando não realizará o desligamento.

## <a name="BKMK_examples"></a>Exemplos

Para forçar os aplicativos para fechar e reiniciar o computador local após um atraso de um minuto, com o motivo "aplicativo: Manutenção (planejada) "e o comentário"Reconfigurando myapp.exe"tipo:
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
Para reiniciar o computador remoto \\ \\ServerName com os mesmos parâmetros, digite:
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
