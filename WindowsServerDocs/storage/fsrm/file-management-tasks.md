---
title: Tarefas de Gerenciamento de Arquivos
description: Este artigo descreve o processo de automatização de tarefas de gerenciamento de arquivo
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4e10aeec47498d6af72e767f519b11ebb4e72932
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961380"
---
# <a name="file-management-tasks"></a>Tarefas de Gerenciamento de Arquivos

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Tarefas de gerenciamento de arquivo automatizam o processo de descoberta de arquivos em um servidor e aplicam comandos simples. Essas tarefas podem ser agendadas para ocorrer periodicamente e reduzir os custos de repetitivos. Arquivos que serão processados por uma tarefa de gerenciamento de arquivo podem ser definidos por meio de qualquer uma das seguintes propriedades:

-   Localização
-   Propriedades de classificação
-   Hora de criação
-   Hora de modificação
-   Hora do último acesso

Tarefas de gerenciamento de arquivo também podem ser configuradas para notificar os proprietários de arquivo de qualquer política iminente que será aplicada aos seus arquivos.

> [!Note]
> Tarefas de gerenciamento de arquivos individuais são executadas em agendamentos independentes.

<br />
Esta seção inclui os tópicos a seguir:

-   [Criar uma tarefa de expiração de arquivos](create-file-expiration-task.md)
-   [Criar uma tarefa de gerenciamento de arquivo personalizada](create-custom-file-management-task.md)

> [!Note]
> Para definir notificações por email e determinados recursos de relatório, você deve configurar primeiro as opções gerais do Gerenciador de recursos do servidor de arquivos.

## <a name="additional-references"></a>Referências adicionais

-   [Definir opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)


