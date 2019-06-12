---
title: Wbadmin start systemstatebackup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d98ba295b2a76baf98e85a01a02677d57922877d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440267"
---
# <a name="wbadmin-start-systemstatebackup"></a>Wbadmin start systemstatebackup



Cria um backup de estado do sistema do computador local e os armazena no local especificado.

> [!NOTE]
> Backup do Windows Server não fazer backup ou recuperação hives do registro de usuário (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou recuperação de estado do sistema.

Para executar um backup de estado do sistema com essa subcomando, você deve ser um membro do **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando**e, em seguida, clique em **executar como administrador**.)

Para obter exemplos de como usar esse subcomando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

## <a name="parameters"></a>Parâmetros

|   Parâmetro   |                                                                                                                                                                                                                      Descrição                                                                                                                                                                                                                      |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -backupTarget | Especifica o local onde você deseja armazenar o backup. O local de armazenamento requer uma letra de unidade ou um volume com base no GUID do formato: \\ \\? \Volume {*GUID*}.</br>Não há suporte para um backup de estado do sistema para uma pasta de rede compartilhada em um computador executando o Windows Server 2008. Se o servidor estiver executando o Windows Server 2008 R2 ou posterior, você pode usar o comando **- backuptarget:\\\\servername\sharedFolder\\**  para armazenar os backups de estado do sistema. |
|    -quiet     |                                                                                                                                                                                                   Executa o subcomando sem prompts para o usuário.                                                                                                                                                                                                    |

## <a name="remarks"></a>Comentários

Para obter informações sobre como salvar um backup de estado do sistema para um volume que, por sua vez, contém os arquivos de estado do sistema, consulte o artigo 944530 na Base de dados de Conhecimento Microsoft ([https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439)).

## <a name="BKMK_examples"></a>Exemplos

Para criar um backup de estado do sistema e armazená-lo no volume f, digite:
```
wbadmin start systemstatebackup -backupTarget:f:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Iniciar WBBackup](https://technet.microsoft.com/library/jj902459.aspx) cmdlet