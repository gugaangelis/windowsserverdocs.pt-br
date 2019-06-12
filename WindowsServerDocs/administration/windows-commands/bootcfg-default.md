---
title: bootcfg default
description: Tópico de comandos do Windows para **bootcfg padrão** -Especifica a entrada de sistema operacional para designar como o padrão.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63aaa7a044634d29c61f3085b1f0c015f4e64444
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434835"
---
# <a name="bootcfg-default"></a>bootcfg default

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica a entrada de sistema operacional para designar como o padrão.

## <a name="syntax"></a>Sintaxe
```
bootcfg /default [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                             Descrição                                                                                              |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                          Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain>\\<User>  | Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual. |
|    /p <Password>     |                                                        Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.                                                         |
| /id <OSEntryLineNum> | Especifica o número de linha de entrada do sistema operacional na seção [operating systems] do arquivo Boot. ini para designar como padrão. A primeira linha depois que o cabeçalho da seção [sistemas operacionais] é 1.  |
|          /?          |                                                                                 Exibe a ajuda no prompt de comando.                                                                                 |

## <a name="BKMK_examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **bootcfg /default**comando:
```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
