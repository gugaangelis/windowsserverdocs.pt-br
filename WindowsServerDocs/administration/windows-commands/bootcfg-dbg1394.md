---
title: bootcfg dbg1394
description: Tópico de comandos do Windows para **bootcfg dbg1394** -1394 configura a porta de depuração para uma entrada de sistema operacional especificado
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85a554e25d1553ea4cd9415bb180df4751966926
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434839"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura a depuração 1394 de porta para uma entrada de sistema operacional especificado.

## <a name="syntax"></a>Sintaxe
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                                                                           Descrição                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   {ON &#124; OFF}    | Especifica o valor para a depuração 1394 de porta.<br /><br />-   **Diante** -habilita o suporte à depuração remota adicionando a opção /dbg1394 especificado <OSEntryLineNum>.<br />-   **DESATIVAR** -desabilita o suporte à depuração remota removendo a opção /dbg1394 especificado <OSEntryLineNum>. |
|    /s <computer>     |                                                                                        Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                                                        |
| /u <Domain>\\<User>  |                                               Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual.                                               |
|    /p <Password>     |                                                                                                      Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.                                                                                                       |
|     /CH channel      |                                                           Especifica o canal a ser usado para depuração. Os valores válidos são números inteiros entre 1 e 64. Não use o **/ch** <Channel> parâmetro se a depuração 1394 de porta está sendo desabilitada.                                                           |
| /id <OSEntryLineNum> |                                  Especifica o número de linha de entrada do sistema operacional na seção [operating systems] do arquivo Boot. ini para o qual as opções de depuração 1394 porta são adicionadas. A primeira linha depois que o cabeçalho da seção [sistemas operacionais] é 1.                                  |
|          /?          |                                                                                                                               Exibe a ajuda no prompt de comando.                                                                                                                               |

## <a name="BKMK_examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **bootcfg /dbg1394**comando:
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
