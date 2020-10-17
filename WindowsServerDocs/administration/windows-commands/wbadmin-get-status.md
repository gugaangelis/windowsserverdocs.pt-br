---
title: wbadmin get status
description: Artigo de referência do comando Wbadmin Get status, que relata o status da operação de backup ou recuperação que está em execução no momento.
ms.topic: reference
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 764c5d3dc808b12488e36af5808acf4c895c5af1
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155946"
---
# <a name="wbadmin-get-status"></a>wbadmin get status

Relata o status da operação de backup ou recuperação que está em execução no momento.

Para obter o status da operação de backup ou recuperação em execução no momento usando este comando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

> [!IMPORTANT]
> Esse comando não é interrompido até que a operação de backup ou recuperação seja concluída. O comando continua a ser executado mesmo se você fechar a janela de comando. Para interromper a operação de backup ou recuperação atual, execute o comando [Wbadmin Stop Job](wbadmin-stop-job.md) .

## <a name="syntax"></a>Syntax

```
wbadmin get status
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin Stop Job](wbadmin-stop-job.md)

- [Get-WBJob](/powershell/module/windowserverbackup/Get-WBJob)
