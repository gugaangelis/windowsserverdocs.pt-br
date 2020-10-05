---
title: shutdown
description: Artigo de referência para o comando de desligamento, que permite desligar ou reiniciar computadores locais ou remotos, um de cada vez.
ms.topic: reference
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 12138ffda4a08f8ac902dbf7c838f0a36627d3cf
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718303"
---
# <a name="shutdown"></a>shutdown

Permite desligar ou reiniciar computadores locais ou remotos, um de cada vez.

## <a name="syntax"></a>Sintaxe

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<computername>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c "comment"]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /i | Exibe a caixa de **desligamento remoto** . A opção **/i** deve ser o primeiro parâmetro após o comando. Se **/i** for especificado, todas as outras opções serão ignoradas. |
| /l | Faz logoff do usuário atual imediatamente, sem período de tempo limite. Você não pode usar **/l** com **/m** ou **/t**. |
| /s | Desliga o computador. |
| /r | Reinicia o computador após o desligamento. |
| /a | Anula um desligamento do sistema. Efetivo somente durante o período de tempo limite. Para usar o **/a**, você também deve usar a opção **/m** . |
| /p | Desliga o computador local somente (não um computador remoto), sem nenhum período de tempo limite ou aviso. Você pode usar **/p** somente com **/d** ou **/f**. Se o computador não oferecer suporte à funcionalidade de desligamento, ele será desligado quando você usar **/p**, mas a potência para o computador permanecerá ativada. |
| /h | Coloca o computador local em hibernação, se a hibernação estiver habilitada. Você pode usar **/h** apenas com **/f**. |
| /e | Permite documentar o motivo do desligamento inesperado no computador de destino. |
| /f | Força o fechamento de aplicativos em execução sem avisar os usuários.<br>**Cuidado:** Usar a opção **/f** pode resultar em perda de dados não salvos. |
| opção `\\<computername>` | Especifica o computador de destino. Não pode ser usado com a opção **/l** . |
| /t `<n>` | Define o período de tempo limite ou o atraso para *n* segundos antes de uma reinicialização ou desligamento. Isso faz com que um aviso seja exibido no console local. Você pode especificar 0-600 segundos. Se você não usar **/t**, o período de tempo limite será de 30 segundos, por padrão. |
| /d `[p | u:]<XX>:<YY>` | Lista o motivo da reinicialização ou desligamento do sistema. Os valores de parâmetro com suporte são:<ul><li>**p** -indica que a reinicialização ou o desligamento está planejado.</li><li>**u** -indica que o motivo é definido pelo usuário.<p>**OBSERVAÇÃO**<br>Se **p** ou **u** não forem especificados, a reinicialização ou o desligamento não será planejado.</li><li>*XX* -especifica o número do motivo principal (um inteiro positivo, menor que 256).</li><li>*AA* Especifica o número do motivo secundário (um inteiro positivo, menor que 65536).</li></ul> |
| /c `<comment>` | Permite que você explique detalhadamente a razão do desligamento. Você deve primeiro fornecer um motivo usando a opção **/d** e deve colocar seus comentários entre aspas. É possível usar até 511 caracteres. |
| /? | Exibe a ajuda no prompt de comando, incluindo uma lista dos motivos principais e secundários definidos no computador local. |

#### <a name="remarks"></a>Comentários

- Os usuários devem ser atribuídos ao direito **de usuário desligar o sistema** para desligar um computador local ou administrado remotamente que está usando o comando de **desligamento** .

- Os usuários devem ser membros do grupo **Administradores** para anotar um desligamento inesperado de um computador local ou administrado remotamente. Se o computador de destino tiver ingressado em um domínio, os membros do grupo **Administradores de domínio** poderão executar esse procedimento. Para obter mais informações, consulte:

  - [Grupos locais padrão](/previous-versions/windows/it-pro/windows-server-2003/cc785098(v=ws.10))

  - [Grupos padrão](/previous-versions/windows/it-pro/windows-server-2003/cc756898(v=ws.10))

- Se você quiser desligar mais de um computador por vez, poderá chamar o **desligamento** para cada computador usando um script ou pode usar **Shutdown** **/i** para exibir a caixa de **desligamento remoto** .

- Se você especificar códigos de motivo principal e secundário, deverá primeiro definir esses códigos de motivo em cada computador em que você planeja usar os motivos. Se os códigos de motivo não estiverem definidos no computador de destino, o controlador de eventos de desligamento não poderá registrar o texto do motivo correto.

- Lembre-se de indicar que um desligamento é planejado usando o parâmetro **p** . Não usar o parâmetro **p** indica que o desligamento não foi planejado.

  - O uso do parâmetro **p** , ao longo do código de motivo de um desligamento não planejado, faz com que o desligamento falhe.

  - Não usar o parâmetro **p** e fornecer apenas o código de motivo para um desligamento planejado, também faz com que o desligamento falhe

## <a name="examples"></a>Exemplos

Para forçar o fechamento de aplicativos e reiniciar o computador local após um atraso de um minuto, com o motivo do *aplicativo: manutenção (planejada)* e o comentário "reconfigurando myapp.exe", digite:

```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

Para reiniciar o computador remoto *myremoteserver* com os mesmos parâmetros do exemplo anterior, digite:

```
shutdown /r /m \\myremoteserver /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
