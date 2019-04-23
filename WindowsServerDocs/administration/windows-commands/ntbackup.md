---
title: ntbackup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 783f73eba2aeaf9f30c5c1e12a623f1f87f24ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846337"
---
# <a name="ntbackup"></a>ntbackup



O **ntbackup** comando não está disponível no Windows Vista ou Windows Server 2008. Em vez disso, você deve usar o **wbadmin** subcomandos para fazer backup e restaurar seu computador e arquivos de um prompt de comando e comando.

Não é possível recuperar backups criados com o **ntbackup** usando o **wbadmin**. No entanto, uma versão do **ntbackup** está disponível como um download para os usuários do Windows Server 2008 e Windows Vista que desejam recuperar backups criados usando **ntbackup**. Esta versão para download **ntbackup** permite executar recuperações somente de backups herdados e ele não pode ser usado em computadores que executam o Windows Server 2008 ou Windows Vista para criar novos backups. Para baixar essa versão do **ntbackup**, consulte [ https://go.microsoft.com/fwlink/?LinkId=82917 ](https://go.microsoft.com/fwlink/?LinkId=82917).

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)