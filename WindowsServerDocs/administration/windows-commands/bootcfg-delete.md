---
title: bootcfg delete
description: Tópico de comandos do Windows para **bootcfg excluir** -exclui uma entrada de sistema operacional na seção de sistemas operacionais do arquivo Boot. ini.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0605e8dfaaf1631da02ac320aa8748d7a34f3fed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840397"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui uma entrada de sistema operacional na seção [operating systems] do arquivo Boot. ini.

## <a name="syntax"></a>Sintaxe
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parâmetros
|Termo|Definição|
|----|-------|
|/s <computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u <Domain>\\<User>|Executa o comando com as permissões de conta do usuário especificado por <User>ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual.|
|/p <Password>|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|/id <OSEntryLineNum>|Especifica o número de linha de entrada do sistema operacional na seção [operating systems] do arquivo Boot. ini para excluir. A primeira linha depois que o cabeçalho da seção [sistemas operacionais] é 1.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="BKMK_examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **bootcfg /delete**comando:
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
