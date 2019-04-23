---
title: bootcfg timeout
description: Tópico de comandos do Windows para **bootcfg timeout** -altera o valor de tempo limite do sistema operacional.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47c68ffaad68ff3e29e8060fdb75adf46165c982
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885517"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o valor de tempo limite do sistema operacional.

## <a name="syntax"></a>Sintaxe
```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/timeout <timeOutValue>|Especifica o valor de tempo limite na seção [carregador de inicialização. O <timeOutValue> é o número de segundos que o usuário deve selecionar um sistema operacional na tela do carregador de inicialização antes de NTLDR carregar o padrão. Intervalo válido para <timeOutValue> é 0 e 999. Se o valor for 0, em seguida, NTLDR iniciará imediatamente o sistema operacional padrão sem exibir a tela do carregador de inicialização.|
|/s <computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u <Domain\User>|Executa o comando com as permissões de conta de usuário especificada por <User> ou < domínio \ usuário >. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual.|
|/p <Password>|Especifica o <Password> da conta de usuário que é especificada na **/u** parâmetro.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="BKMK_examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **bootcfg /timeout** comando:
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
