---
title: bootcfg delete
description: Tópico de comandos do Windows para **bootcfg delete** – exclui uma entrada do sistema operacional na seção sistemas operacionais do arquivo boot. ini.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d2298c07af32e66a2ffcebb85ec780da762be58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380012"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

exclui uma entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini.

## <a name="syntax"></a>Sintaxe
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parâmetros

|         Termo         |                                                                                             Definição                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain> @ no__t-1 @ no__t-2  | Executa o comando com as permissões de conta do usuário especificado por <User>or <Domain> @ no__t-2 @ no__t-3. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
|    /p <Password>     |                                                        Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                        |
| /ID <OSEntryLineNum> |        Especifica o número da linha da entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini a ser excluído. A primeira linha após o cabeçalho da seção [Operating Systems] é 1.        |
|          /?          |                                                                                Exibe a ajuda no prompt de comando.                                                                                 |

## <a name="BKMK_examples"></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **bootcfg/delete**:
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
