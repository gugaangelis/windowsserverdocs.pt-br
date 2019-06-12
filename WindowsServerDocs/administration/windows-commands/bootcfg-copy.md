---
title: bootcfg copy
description: Tópico de comandos do Windows para **bootcfg cópia** -faz uma cópia de uma entrada de inicialização existente, ao qual você pode adicionar opções de linha de comando.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b76ecfe953d1a462e311fdaaeba35e8f962165c4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434866"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz uma cópia de uma entrada de inicialização existente, ao qual você pode adicionar opções de linha de comando.

## <a name="syntax"></a>Sintaxe
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                             Descrição                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain>\\<User>  | Executa o comando com as permissões de conta do usuário especificado por <User>ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual. |
|    /p <Password>     |                                                        Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.                                                        |
|   /d <Description>   |                                                                    Especifica a descrição para a nova entrada de sistema operacional.                                                                    |
| /id <OSEntryLineNum> |         Especifica o número de linha de entrada do sistema operacional na seção [operating systems] do arquivo Boot. ini para copiar. A primeira linha depois que o cabeçalho da seção [sistemas operacionais] é 1.         |
|          /?          |                                                                                Exibe a ajuda no prompt de comando.                                                                                 |

## <a name="BKMK_examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **bootcfg /copy** comando para copiar a entrada de inicialização 1 e insira "\ABC Server\\" como a descrição:
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
