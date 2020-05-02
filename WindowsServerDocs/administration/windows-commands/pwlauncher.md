---
title: pwlauncher
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ddda3ab7831643c3c2c096ae87893d97f90155c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722748"
---
# <a name="pwlauncher"></a>pwlauncher



Habilita ou desabilita as opções de inicialização do Windows to go (pwlauncher). A ferramenta de linha de comando **pwlauncher** permite configurar o computador para ser inicializado em um espaço de trabalho do Windows to go automaticamente (supondo que um esteja presente), sem exigir que você insira seu firmware ou altere suas opções de inicialização.



## <a name="syntax"></a>Sintaxe

```
Pwlauncher {/enable | /disable}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/Enable|Habilita as opções de inicialização do Windows to go para que o computador seja inicializado automaticamente de um dispositivo USB quando estiver presente|
|/Disable|Desabilita as opções de inicialização do Windows to go para que o computador não possa ser inicializado a partir de um dispositivo USB, a menos que seja configurado manualmente no firmware.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

O maior obstáculo para um usuário que deseja usar o Windows to go é colocar o computador em inicialização por meio do USB. Isso é tradicionalmente feito inserindo o firmware e experimentando opções de configuração diferentes até que o computador seja configurado corretamente. Essa não é uma tarefa simples para a maioria dos usuários e é extremamente arriscada, pois o firmware contém opções que podem tornar um sistema inutilizável se usado incorretamente. Para ajudar a aliviar esse problema, os sistemas operacionais Windows 8and posteriores incluem um recurso chamado opções de inicialização do Windows to go que permite que um usuário configure seu computador para inicializar do USB a partir do Windows, sem precisar inserir o firmware, contanto que seu firmware dê suporte à inicialização do USB. A habilitação de um sistema para sempre inicializar do USB primeiro tem implicações que você deve considerar. Por exemplo, um dispositivo USB que inclui malware pode ser inicializado inadvertidamente para comprometer o sistema, ou várias unidades USB podem ser conectadas para causar um conflito de inicialização. Por esse motivo, a configuração padrão tem as opções de inicialização do Windows to go desabilitadas por padrão. Além disso, são necessários privilégios de administrador para configurar as opções de inicialização do Windows to go. Se você habilitar as opções de inicialização do Windows to go usando a ferramenta de linha de comando pwlauncher ou o aplicativo **alterar as opções de inicialização do Windows to go** , o computador tentará inicializar a partir de qualquer dispositivo USB inserido no computador antes de ser iniciado.

## <a name="examples"></a>Exemplos

Para mostrar como você pode usar o comando **pwlauncher** para habilitar a inicialização do USB:
```
Pwlauncher /enable
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)