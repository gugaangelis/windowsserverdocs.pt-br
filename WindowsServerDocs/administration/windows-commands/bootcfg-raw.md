---
title: bootcfg raw
description: Tópico de comandos do Windows para **bootcfg bruto** -adiciona opções de carregamento do sistema operacional especificadas como uma cadeia de caracteres a uma entrada de sistema operacional no **[sistemas operacionais]** seção do arquivo Boot. ini.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a68c59eb3a7018ba8a7c0c96b594f0ed68d914b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841597"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona as opções de carregamento do sistema operacional especificadas como uma cadeia de caracteres a uma entrada de sistema operacional no **[sistemas operacionais]** seção do arquivo Boot. ini.

## <a name="syntax"></a>Sintaxe
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
## <a name="parameters"></a>Parâmetros
|Termo|Definição|
|----|-------|
|/s <computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u <Domain> \\<User>|Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual.|
|/p <Password>|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|<OSLoadOptionsString>|Especifica as opções de carregamento do sistema operacional para adicionar a entrada do sistema operacional. Essas opções de carregamento substituirá quaisquer opções de carga existentes associadas à entrada do sistema operacional. Nenhuma validação de <OSLoadOptions> é feita.|
|/id <OSEntryLineNum>|Especifica o número de linha de entrada do sistema operacional na seção [operating systems] do arquivo Boot. ini para atualizar. A primeira linha depois que o cabeçalho da seção [sistemas operacionais] é 1.|
|/a|Especifica que as opções de sistema operacional que está sendo adicionadas devem ser anexadas a quaisquer opções de sistema operacional existente.|
|/?|Exibe a ajuda no prompt de comando.|
##### <a name="remarks"></a>Comentários
-   **BOOTCFG bruto** é usado para adicionar texto ao final de uma entrada de sistema operacional, substituindo quaisquer opções de entrada do sistema operacional existente. Esse texto deve conter as opções de carregamento do sistema operacional válido, como **/Debug**, **/fastdetect**, **apenas**, **/baudrate**, **/ /crashdebug**, e **/sos**. Por exemplo, o comando a seguir adiciona "**/debug /fastdetect**" ao final da primeira entrada de sistema operacional, substituindo quaisquer opções de entrada do sistema operacional anterior:
    ```
    bootcfg /raw "/debug /fastdetect" /id 1
    ```
## <a name="BKMK_examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **bootcfg / brutos** comando:
```
bootcfg /raw "/debug /sos" /id 2
bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 "/crashdebug " /id 2
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
