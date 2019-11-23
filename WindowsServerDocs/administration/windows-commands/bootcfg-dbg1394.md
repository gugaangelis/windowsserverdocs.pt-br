---
title: bootcfg dbg1394
description: Tópico de comandos do Windows para **Bootcfg dbg1394** – configura a depuração de porta 1394 para uma entrada de sistema operacional especificada
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8550871c60343fdc6d797f3f81729c24270400b4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380093"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura a depuração de porta 1394 para uma entrada de sistema operacional especificada.

## <a name="syntax"></a>Sintaxe
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                                                                           Descrição                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   {ON &#124; off}    | Especifica o valor para a depuração de porta 1394.<br /><br />-   **habilita o suporte à depuração** remota adicionando a opção/dbg1394 ao <OSEntryLineNum>especificado.<br />-   **desativado** -desabilita o suporte à depuração remota removendo a opção/dbg1394 da <OSEntryLineNum>especificada. |
|    /s <computer>     |                                                                                        Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                                                        |
| /u <Domain>\\<User>  |                                               Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.                                               |
|    /p <Password>     |                                                                                                      Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                                                       |
|     /CH canal      |                                                           Especifica o canal a ser usado para depuração. Os valores válidos são inteiros entre 1 e 64. Não use o parâmetro **/ch** <Channel> se a depuração de porta 1394 estiver sendo desabilitada.                                                           |
| /ID <OSEntryLineNum> |                                  Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini para o qual as opções de depuração de porta 1394 são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1.                                  |
|          /?          |                                                                                                                               Exibe a ajuda no prompt de comando.                                                                                                                               |

## <a name="BKMK_examples"></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/dbg1394**:
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
