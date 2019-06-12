---
title: bootcfg debug
description: Tópico de comandos do Windows para **bootcfg depuração** - adiciona ou altera as configurações de depuração de uma entrada de sistema operacional especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 44a1145384a62d30f055cb48fd7ed6adccd2c69b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434827"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona ou altera as configurações de depuração para uma entrada de sistema operacional especificado.

## <a name="syntax"></a>Sintaxe
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parâmetros

|                           Parâmetro                           |                                                                                                                                                                                                                    Descrição                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {ON &#124; OFF&#124; edit}                   | Especifica o valor para a depuração.<br /><br />**Diante** -habilita o suporte à depuração remota adicionando a opção /debug especificado <OSEntryLineNum>.<br /><br />**DESATIVAR** -desabilita o suporte à depuração remota, removendo a opção /debug do especificado <OSEntryLineNum>.<br /><br />**Edite** -permite que as alterações transmissão e porta de configurações de taxa, alterando os valores associados com a opção /debug especificado <OSEntryLineNum>. |
|                         /s <computer>                         |                                                                                                                                                                Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                                                                                                                                 |
|                      /u <Domain>\\<User>                      |                                                                                                                       Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual.                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.                                                                                                                                                                               |
|       /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4}        |                                                                                                                                                                Especifica a porta COM a ser usada para depuração. Não use o **/porta** parâmetro se a depuração está sendo desativado.                                                                                                                                                                |
| /baud {9600&#124; 19200&#124; 38400&#124; 57600&#124; 115200} |                                                                                                                                                               Especifica a taxa de transmissão a ser usada para depuração. Não use o **/baud** parâmetro se a depuração está sendo desativado.                                                                                                                                                                |
|                     /id <OSEntryLineNum>                      |                                                                                                               Especifica o número de linha de entrada do sistema operacional na seção [operating systems] do arquivo Boot. ini para o qual as opções de depuração são adicionadas. A primeira linha depois que o cabeçalho da seção [sistemas operacionais] é 1.                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       Exibe a ajuda no prompt de comando.                                                                                                                                                                                                        |

##### <a name="remarks"></a>Comentários
- Se a depuração 1394 porta for necessária, use [bootcfg dbg1394](bootcfg-dbg1394.md).
  ## <a name="BKMK_examples"></a>Exemplos
  Os exemplos a seguir mostram como você pode usar o **bootcfg /debug**comando:
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  #### <a name="additional-references"></a>Referências adicionais
  [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
