---
title: pwlauncher
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1aa2e22f74323bb6cabfc644ca67e17a7fcbd3fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851017"
---
# <a name="pwlauncher"></a>pwlauncher



Habilita ou desabilita o Windows para acessar opções de inicialização (pwlauncher). O **pwlauncher** ferramenta de linha de comando permite que você configure o computador para inicializar automaticamente em um espaço de trabalho do Windows To Go (supondo que um estiver presente), sem exigir que você insira seu firmware ou altere as opções de inicialização.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Pwlauncher {/enable | /disable}
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/enable|Habilita opções de inicialização do Windows To Go para que o computador será automaticamente inicializado de um dispositivo USB, quando presente|
|/disable|Desabilita as opções de inicialização do Windows To Go para que o computador não pode ser inicializado a partir de um dispositivo USB, a menos que configurado manualmente no firmware.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

O maior obstáculo para um usuário que desejarem usar o Windows To Go é obter o seu computador para inicializar de USB. Tradicionalmente, isso é feito inserindo o firmware e experimentar diferentes opções de configuração até que o computador está configurado corretamente. Isso não é uma tarefa simple para a maioria dos usuários e é muito arriscado porque o firmware contém opções que podem inutilizar um sistema se usada incorretamente. Para ajudar a aliviar esse problema, o Windows 8and sistemas operacionais posteriores incluem um recurso chamado "Windows para acessar as opções de inicialização? que permite que um usuário configurar seu computador para inicializar de USB de dentro do Windows — sem inserir nunca firmware, desde que o firmware dá suporte à inicialização a partir de USB. Primeiro permitindo que um sistema sempre inicializar de USB tem implicações que devem ser consideradas. Por exemplo, um dispositivo USB que inclui o malware poderia ser inicializado inadvertidamente para comprometer o sistema ou várias unidades USB podem ser conectadas para fazer com que um conflito de inicialização. Por esse motivo, a configuração padrão tem o Windows para opções de inicialização Go desabilitado por padrão. Além disso, são necessários privilégios de administrador para configurar o Windows para opções de inicialização Go. Se você habilitar as opções de inicialização do Windows To Go usando a ferramenta de linha de comando pwlauncher ou o **alteração Windows para acessar opções de inicialização** o computador tentará inicializar a partir de qualquer dispositivo USB que é inserido no computador antes que seja do aplicativo iniciado.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir mostra como você pode usar o **pwlauncher** comando para habilitar a inicialização a partir do USB:
```
Pwlauncher /enable
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)