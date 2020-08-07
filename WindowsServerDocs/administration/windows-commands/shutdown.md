---
title: shutdown
description: Artigo de referência para desligamento, que permite desligar ou reiniciar computadores locais ou remotos um de cada vez.
ms.topic: article
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8dff8150cb6ccfea24238567581320a9b11650d3
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882365"
---
# <a name="shutdown"></a>shutdown

Permite desligar ou reiniciar computadores locais ou remotos um por vez.



## <a name="syntax"></a>Sintaxe

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c comment]]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/i|Exibe a caixa de **diálogo de desligamento remoto** . A opção **/i** deve ser o primeiro parâmetro após o comando. Se **/i** for especificado, todas as outras opções serão ignoradas.|
|/l|Faz logoff do usuário atual imediatamente, sem período de tempo limite. Você não pode usar **/l** com **/m** ou **/t**.|
|/s|Desliga o computador.|
|/r|Reinicia o computador após o desligamento.|
|/a|Anula um desligamento do sistema. Efetivo somente durante o período de tempo limite. Para usar o **/a**, você também deve usar a opção **/m** .|
|/p|Desliga o computador local somente (não um computador remoto), sem nenhum período de tempo limite ou aviso. Você pode usar **/p** somente com **/d** ou **/f**. Se o seu computador não oferecer suporte à funcionalidade de desligamento, ele será desligado quando você usar **/p**, mas a potência para o computador permanecerá ativada.|
|/h|Coloca o computador local em hibernação, se a hibernação estiver habilitada. Você pode usar **/h** apenas com **/f**.|
|/e|Permite documentar o motivo do desligamento inesperado no computador de destino.|
|/f|Força o fechamento de aplicativos em execução sem avisar os usuários.</br>Cuidado: usar a opção **/f** pode resultar em perda de dados não salvos.|
|opção\\\\\<ComputerName>|Especifica o computador de destino. Não pode ser usado com a opção **/l** .|
|/t\<XXX>|Define o período de tempo limite ou o atraso para *xxx* segundos antes de uma reinicialização ou desligamento. Isso faz com que um aviso seja exibido no console local. Você pode especificar 0-600 segundos. Se você não usar **/t**, o período de tempo limite será de 30 segundos por padrão.|
|/d [p \| u:] \<XX> :\<YY>|Lista o motivo da reinicialização ou desligamento do sistema. Estes são os valores de parâmetro:</br>**p** indica que a reinicialização ou o desligamento está planejado.</br>**u** indica que o motivo é definido pelo usuário.</br>Observação: se **p** ou **u** não forem especificados, a reinicialização ou o desligamento não será planejado.</br>*XX* especifica o número do motivo principal (inteiro positivo menor que 256).</br>*AA* Especifica o número do motivo secundário (inteiro positivo menor que 65536).|
|/c\<Comment>|Permite que você explique detalhadamente a razão do desligamento. Você deve primeiro fornecer um motivo usando a opção **/d** . Você deve incluir comentários entre aspas. É possível usar até 511 caracteres.|
|/?|Exibe a ajuda no prompt de comando, incluindo uma lista dos motivos principais e secundários definidos no computador local.|

## <a name="remarks"></a>Comentários

-   Os usuários devem ser atribuídos ao direito **de usuário desligar o sistema** para desligar um computador local ou administrado remotamente que está usando o comando de **desligamento** .
-   Os usuários devem ser membros do grupo Administradores para anotar um desligamento inesperado de um computador local ou administrado remotamente. Se o computador de destino tiver ingressado em um domínio, os membros do grupo Administradores de domínio poderão executar esse procedimento. Para obter mais informações, consulte:
    -   [Grupos locais padrão](/previous-versions/windows/it-pro/windows-server-2003/cc785098(v=ws.10))
    -   [Grupos padrão](/previous-versions/windows/it-pro/windows-server-2003/cc756898(v=ws.10))
-   Se você quiser desligar mais de um computador por vez, poderá chamar o **desligamento** para cada computador usando um script ou pode usar **Shutdown** **/i** para exibir a caixa de diálogo de desligamento remoto.
-   Se você especificar códigos de motivo principal e secundário, deverá primeiro definir esses códigos de motivo em cada computador em que você planeja usar os motivos. Se os códigos de motivo não estiverem definidos no computador de destino, o controlador de eventos de desligamento não poderá registrar o texto de motivo correto.
-   Lembre-se de indicar que um desligamento é planejado usando o parâmetro **p:** . Omitir **p:** indica que um desligamento não está planejado. Se você digitar **p:** seguido pelo código de motivo para um desligamento não planejado, o comando não realizará o desligamento. Por outro lado, se você omitir **p:** e digitar o código de motivo para um desligamento planejado, o comando não realizará o desligamento.

## <a name="examples"></a>Exemplos

Para forçar que os aplicativos fechem e reiniciem o computador local após um atraso de um minuto com o motivo do aplicativo: manutenção (planejada) e o comentário reconfigurando myapp.exe tipo:
```
shutdown /r /t 60 /c Reconfiguring myapp.exe /f /d p:4:1
```
Para reiniciar o ServerName do computador remoto \\ \\ com os mesmos parâmetros, digite:
```
shutdown /r /m \\servername /t 60 /c Reconfiguring myapp.exe /f /d p:4:1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
