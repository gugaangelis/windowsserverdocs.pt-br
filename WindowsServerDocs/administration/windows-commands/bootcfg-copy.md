---
title: bootcfg copy
description: O tópico de comandos do Windows para **Bootcfg Copy** – faz uma cópia de uma entrada de inicialização existente, à qual você pode adicionar opções de linha de comando.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42a408443cbe6722c25780f7c27d70b05da7eb8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380120"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz uma cópia de uma entrada de inicialização existente, à qual você pode adicionar opções de linha de comando.

## <a name="syntax"></a>Sintaxe
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                             Descrição                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain>\\<User>  | Executa o comando com as permissões de conta do usuário especificado por <User>ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
|    /p <Password>     |                                                        Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                        |
|   /d <Description>   |                                                                    Especifica a descrição para a nova entrada do sistema operacional.                                                                    |
| /ID <OSEntryLineNum> |         Especifica o número da linha da entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini a ser copiado. A primeira linha após o cabeçalho da seção [Operating Systems] é 1.         |
|          /?          |                                                                                Exibe a ajuda no prompt de comando.                                                                                 |

## <a name="BKMK_examples"></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/Copy** para copiar a entrada de inicialização 1 e inserir "\ABC Server\\" como a descrição:
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
