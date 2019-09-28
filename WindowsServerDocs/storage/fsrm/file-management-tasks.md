---
title: Tarefas de Gerenciamento de Arquivos
description: Este artigo descreve o processo de automatização de tarefas de gerenciamento de arquivo
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 257ee2955c4f521d14f01ec197fd45e5194eef02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394096"
---
# <a name="file-management-tasks"></a>Tarefas de Gerenciamento de Arquivos

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Tarefas de gerenciamento de arquivo automatizam o processo de descoberta de arquivos em um servidor e aplicam comandos simples. Essas tarefas podem ser agendadas para ocorrer periodicamente e reduzir os custos de repetitivos. Arquivos que serão processados por uma tarefa de gerenciamento de arquivo podem ser definidos por meio de qualquer uma das seguintes propriedades:

-   Location
-   Propriedades de classificação
-   Hora de criação
-   Hora de modificação
-   Hora do último acesso

Tarefas de gerenciamento de arquivo também podem ser configuradas para notificar os proprietários de arquivo de qualquer política iminente que será aplicada aos seus arquivos.

> [!Note]
> Tarefas de gerenciamento de arquivos individuais são executadas em agendamentos independentes.

<br />
Esta seção inclui os seguintes tópicos:

-   [Criar uma tarefa de expiração de arquivos](create-file-expiration-task.md)
-   [Criar uma tarefa de gerenciamento de arquivo personalizada](create-custom-file-management-task.md)

> [!Note]
> Para definir notificações por email e determinados recursos de relatório, você deve configurar primeiro as opções gerais do Gerenciador de recursos do servidor de arquivos.

## <a name="see-also"></a>Consulte também

-   [Definir opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)


