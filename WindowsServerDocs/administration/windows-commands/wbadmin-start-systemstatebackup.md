---
title: Wbadmin start systemstatebackup
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0244f984d29c8a802475d2dc08f1cdfe4495f0b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362234"
---
# <a name="wbadmin-start-systemstatebackup"></a>Wbadmin start systemstatebackup



Cria um backup de estado do sistema do computador local e o armazena no local especificado.

> [!NOTE]
> Backup do Windows Server não faz backup ou recupera hives de usuário do registro (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou da recuperação do estado do sistema.

Para executar um backup de estado do sistema com esse subcomando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

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
| -backupTarget | Especifica o local em que você deseja armazenar o backup. O local de armazenamento requer uma letra da unidade ou um volume baseado em GUID do formato: \\ @ no__t-1? \Volume{*GUID*}.</br>Não há suporte para um backup de estado do sistema em uma pasta de rede compartilhada em um computador que esteja executando o Windows Server 2008. Se o servidor estiver executando o Windows Server 2008 R2 ou posterior, você poderá usar o comando **-backuptarget: \\ @ no__t-2servername\sharedFolder @ no__t-3** para armazenar os backups de estado do sistema. |
|    -quiet     |                                                                                                                                                                                                   Executa o subcomando sem prompts para o usuário.                                                                                                                                                                                                    |

## <a name="remarks"></a>Comentários

Para obter informações sobre como salvar um backup de estado do sistema em um volume que, por sua vez, contém arquivos de estado do sistema, consulte o artigo 944530 na base de dados de conhecimento Microsoft ([https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439)).

## <a name="BKMK_examples"></a>Disso

Para criar um backup de estado do sistema e armazená-lo no volume f, digite:
```
wbadmin start systemstatebackup -backupTarget:f:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Start-WBBackup](https://technet.microsoft.com/library/jj902459.aspx)