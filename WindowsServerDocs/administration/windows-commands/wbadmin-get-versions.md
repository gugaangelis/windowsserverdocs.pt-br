---
title: wbadmin get versions
description: Artigo de referência para WBADMIN Get Versions, que lista detalhes sobre os backups disponíveis que são armazenados no computador local ou em outro computador.
ms.topic: article
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 209244f18758a13838a27ffd2f44e871cf5971c1
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891728"
---
# <a name="wbadmin-get-versions"></a>wbadmin get versions



Lista detalhes sobre os backups disponíveis que são armazenados no computador local ou em outro computador. Quando esse subcomando é usado sem parâmetros, ele lista todos os backups do computador local, mesmo que esses backups não estejam disponíveis. Os detalhes fornecidos para um backup incluem o tempo de backup, o local de armazenamento de backup, o identificador de versão (necessário para o subcomando **Wbadmin Get Items** e para executar recuperações) e o tipo de recuperações que você pode executar.

Para obter detalhes sobre os backups disponíveis usando esse subcomando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um **prompt de comando** de prompt de comandos com privilégios elevados e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-backupTarget|Especifica o local de armazenamento que contém os backups dos quais você deseja obter os detalhes. Use para listar backups armazenados nesse local de destino. Os locais de destino de backup podem ser unidades de disco conectadas localmente, volumes, pastas compartilhadas remotas, mídia removível, como unidades de DVD ou outras mídias ópticas. Se o **Wbadmin Get Versions** for executado no mesmo computador em que o backup foi criado, esse parâmetro não será necessário. No entanto, esse parâmetro é necessário para obter informações sobre um backup criado a partir de outro computador.|
|-computador|Especifica o computador para o qual você deseja obter detalhes de backup. Use quando os backups de vários computadores estiverem armazenados no mesmo local. Deve ser usado quando **-backupTarget** é especificado.|

## <a name="remarks"></a>Comentários

Para listar itens disponíveis para recuperação de um backup específico, use **Wbadmin Get Items**.

## <a name="examples"></a>Exemplos

Para ver uma lista de backups disponíveis que são armazenados no volume h, digite:
```
wbadmin get versions -backupTarget:h:
```
Para ver uma lista de backups disponíveis que são armazenados na pasta compartilhada remota \\ \\ servername\share para o computador Server01, digite:
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBBackupTarget](/powershell/module/windowserverbackup/?view=winserver2012r2-ps)
