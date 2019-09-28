---
title: bootcfg debug
description: O tópico de comandos do Windows para **Bootcfg Debug** – adiciona ou altera as configurações de depuração para uma entrada de sistema operacional especificada.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6659cf2bfdf83b1b2fe6f6c811365775526768a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380047"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona ou altera as configurações de depuração para uma entrada de sistema operacional especificada.

## <a name="syntax"></a>Sintaxe
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parâmetros

|                           Parâmetro                           |                                                                                                                                                                                                                    Descrição                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {ON &#124; off&#124; Edit}                   | Especifica o valor para depuração.<br /><br />Habilita o **suporte à depuração** remota adicionando a opção/debug ao <OSEntryLineNum> especificado.<br /><br />**Off** -desabilita o suporte à depuração remota removendo a opção/debug do <OSEntryLineNum> especificado.<br /><br />**Editar** – permite alterações nas configurações de porta e taxa de transmissão alterando os valores associados à opção/debug para o <OSEntryLineNum> especificado. |
|                         /s <computer>                         |                                                                                                                                                                Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                                                                                                                                 |
|                      /u <Domain> @ no__t-1 @ no__t-2                      |                                                                                                                       Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> @ no__t-2 @ no__t-3. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                                                                                                                               |
|       /Port {COM1 &#124; COM2 &#124; com3 &#124; COM4}        |                                                                                                                                                                Especifica a porta COM a ser usada para depuração. Não use o parâmetro **/Port** se a depuração estiver sendo desabilitada.                                                                                                                                                                |
| /baud {9600&#124; 19200&#124; 38400&#124; 57600&#124; 115200} |                                                                                                                                                               Especifica a taxa de transmissão a ser usada para depuração. Não use o parâmetro **/baud** se a depuração estiver sendo desabilitada.                                                                                                                                                                |
|                     /ID <OSEntryLineNum>                      |                                                                                                               Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini ao qual as opções de depuração são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1.                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       Exibe a ajuda no prompt de comando.                                                                                                                                                                                                        |

##### <a name="remarks"></a>Comentários
- se a depuração de porta 1394 for necessária, use [Bootcfg dbg1394](bootcfg-dbg1394.md).
  ## <a name="BKMK_examples"></a>Disso
  Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/Debug**:
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  #### <a name="additional-references"></a>Referências adicionais
  [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
