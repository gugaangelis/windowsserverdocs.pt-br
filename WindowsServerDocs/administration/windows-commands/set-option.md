---
title: Opção Set
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81678768bb2b5fcfd7f2f2d067562e78e93dc549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848847"
---
# <a name="set-option"></a>Opção Set



Define as opções para criação de cópias de sombra. Se usado sem parâmetros, **defina a opção** exibe a Ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[differential | plex]|Especifica o tipo de cópia de sombra para criar para o provedor.|
|[transportáveis]|Especifica que a cópia de sombra não deve ser importada. O arquivo. cab de metadados mais tarde pode ser usado para importar a cópia de sombra para o mesmo ou em outro computador.|
|[rollbackrecover]|Sinaliza usassem *AutoRecuperação* durante a **PostSnapshot** eventos. Isso é útil se a cópia de sombra será usada para reversão (por exemplo, com a mineração de dados).|
|[txfrecover]|Solicitações de VSS para fazer a cópia de sombra transacionalmente consistente durante a criação.|
|[noautorecover]|Gravadores é interrompida e o sistema de arquivos de realizar qualquer alteração de recuperação para a cópia de sombra para um estado transacionalmente consistente. **Noautorecover** não pode ser usado com **txfrecover** ou **rollbackrecover**.|

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)