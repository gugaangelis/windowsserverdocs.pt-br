---
title: reset
description: Artigo de referência para o comando Reset, que redefine DiskShadow.exe para o estado padrão.
ms.topic: reference
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 84f4aedee746e642e59a09055c3994160f503afa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626888"
---
# <a name="reset"></a>reset

Redefine DiskShadow.exe para o estado padrão. Esse comando é especialmente útil para separar operações de DiskShadow compostos, como **criar**, **importar**, **fazer backup**ou **restaurar**.

> [! IMPORTANTE depois de executar esse comando, você perderá informações de estado de comandos, como **Adicionar**, **definir**, **carregar** **ou gravar**. Esse comando também libera as interfaces IVssBackupComponent e perde as cópias de sombra não persistentes.

## <a name="syntax"></a>Sintaxe

```
reset
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Create](create.md)

- [comando de importação](import_1.md)

- [comando de backup](begin-backup.md)

- [comando Restore](begin-restore.md)

- [Adicionar comando](add.md)

- [comando Set](set_2.md)

- [carregar comando](reg-load.md)

- [comando do gravador](writer.md)
