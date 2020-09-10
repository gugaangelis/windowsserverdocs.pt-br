---
title: pwlauncher
description: Artigo de referência para o comando pwlauncher, que habilita ou desabilita as opções de inicialização do Windows to go (pwlauncher).
ms.topic: reference
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8bdc4f7b3b5c91cb804f6760a60ec421652d1642
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639924"
---
# <a name="pwlauncher"></a>pwlauncher

Habilita ou desabilita as opções de inicialização do Windows to go (pwlauncher). A ferramenta de linha de comando **pwlauncher** permite configurar o computador para ser inicializado em um espaço de trabalho do Windows to go automaticamente (supondo que um esteja presente), sem exigir que você insira seu firmware ou altere suas opções de inicialização.

As opções de inicialização do Windows to go permitem que um usuário configure seu computador para inicializar de USB de dentro do Windows, sem precisar inserir o firmware, contanto que seu firmware dê suporte à inicialização a partir do USB. A habilitação de um sistema para sempre inicializar do USB primeiro tem implicações que você deve considerar. Por exemplo, um dispositivo USB que inclui malware pode ser inicializado inadvertidamente para comprometer o sistema, ou várias unidades USB podem ser conectadas para causar um conflito de inicialização. Por esse motivo, a configuração padrão tem as opções de inicialização do Windows to go desabilitadas por padrão. Além disso, são necessários privilégios de administrador para configurar as opções de inicialização do Windows to go. Se você habilitar as opções de inicialização do Windows to go usando a ferramenta de linha de comando pwlauncher ou o aplicativo **alterar as opções de inicialização do Windows to go** , o computador tentará inicializar a partir de qualquer dispositivo USB inserido no computador antes de ser iniciado.

## <a name="syntax"></a>Sintaxe

```
pwlauncher {/enable | /disable}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /Enable | Habilita as opções de inicialização do Windows to go para que o computador seja inicializado automaticamente de um dispositivo USB quando presente. |
| /Disable | Desabilita as opções de inicialização do Windows to go, portanto, o computador não pode ser inicializado a partir de um dispositivo USB, a menos que seja configurado manualmente no firmware. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para habilitar a inicialização do USB:

```
pwlauncher /enable
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
