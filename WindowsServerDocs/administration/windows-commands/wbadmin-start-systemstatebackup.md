---
title: wbadmin start systemstatebackup
description: Artigo de referência para Wbadmin start systemstatebackup, que cria um backup de estado do sistema do computador local e o armazena no local especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e92c508d2c9ca9ae759d81292ec3d511144dbd7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930908"
---
# <a name="wbadmin-start-systemstatebackup"></a>wbadmin start systemstatebackup



Cria um backup de estado do sistema do computador local e o armazena no local especificado.

> [!NOTE]
> Backup do Windows Server não faz backup ou recupera hives de usuário do registro (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou da recuperação do estado do sistema.

Para executar um backup de estado do sistema com esse subcomando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

### <a name="parameters"></a>Parâmetros

|   Parâmetro   |                                                                                                                                                                                                                      Descrição                                                                                                                                                                                                                      |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -backupTarget | Especifica o local em que você deseja armazenar o backup. O local de armazenamento requer uma letra da unidade ou um volume baseado em GUID do formato: \\ \\ ? \Volume{*GUID*}.</br>Não há suporte para um backup de estado do sistema em uma pasta de rede compartilhada em um computador que esteja executando o Windows Server 2008. Se o servidor estiver executando o Windows Server 2008 R2 ou posterior, você poderá usar o comando **-backuptarget: \\ \\ servername\sharedFolder \\ ** para armazenar os backups de estado do sistema. |
|    -quiet     |                                                                                                                                                                                                   Executa o subcomando sem prompts para o usuário.                                                                                                                                                                                                    |

## <a name="remarks"></a>Comentários

Para obter informações sobre como salvar um backup de estado do sistema em um volume que, por sua vez, contém arquivos de estado do sistema, consulte o artigo 944530 na base de dados de conhecimento Microsoft ( [https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439) ).

## <a name="examples"></a>Exemplos

Para criar um backup de estado do sistema e armazená-lo no volume f, digite:
```
wbadmin start systemstatebackup -backupTarget:f:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Start-WBBackup](https://technet.microsoft.com/library/jj902459.aspx)