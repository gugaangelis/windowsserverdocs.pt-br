---
title: bootcfg delete
description: O tópico de comandos do Windows para bootcfg delete, que exclui uma entrada do sistema operacional na seção sistemas operacionais do arquivo boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01ec7a4dde1e22982f2cf0fa30245c33e09cc0ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848539"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui uma entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini.

## <a name="syntax"></a>Sintaxe
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Parâmetros

|         Termo         |                                                                                             Definição                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain>\\<User>  | Executa o comando com as permissões de conta do usuário especificado por <User>ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
|    /p <Password>     |                                                        Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                        |
| /ID <OSEntryLineNum> |        Especifica o número da linha da entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini a ser excluído. A primeira linha após o cabeçalho da seção [Operating Systems] é 1.        |
|          /?          |                                                                                Exibe a ajuda no prompt de comando.                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **bootcfg/delete**:
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
