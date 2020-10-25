---
title: wbadmin start systemstatebackup
description: Artigo de referência para o comando Wbadmin start systemstatebackup, que cria um backup de estado do sistema do computador local e o armazena no local especificado.
ms.topic: reference
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: afdfb3f4a52ae0f5897517f8d59069bea820bc4a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524721"
---
# <a name="wbadmin-start-systemstatebackup"></a>wbadmin start systemstatebackup

Cria um backup de estado do sistema do computador local e o armazena no local especificado.

Para executar um backup de estado do sistema usando esse comando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

> [!NOTE]
> Backup do Windows Server não faz backup ou recupera hives de usuário do registro (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou da recuperação do estado do sistema.

## <a name="syntax"></a>Sintaxe

```
wbadmin start systemstatebackup -backupTarget:<VolumeName> [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -backupTarget | Especifica o local em que você deseja armazenar o backup. O local de armazenamento requer uma letra da unidade ou um volume baseado em GUID do formato: `\\?\Volume{*GUID*}` . Use o comando `-backuptarget:\\servername\sharedfolder\` para armazenar backups de estado do sistema. |
| -quiet | Executa o comando sem prompts para o usuário. |

## <a name="examples"></a>Exemplos

Para criar um backup de estado do sistema e armazená-lo no volume f, digite:

```
wbadmin start systemstatebackup -backupTarget:f:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [Start-WBBackup](/powershell/module/windowserverbackup/Start-WBBackup)
