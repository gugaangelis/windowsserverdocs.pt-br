---
title: Tarefas de Gerenciamento de Arquivos
description: "Este artigo descreve o processo de automatização de tarefas de gerenciamento de arquivo"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e83d0b79117144d42a0aff748f482f3c181cb300
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="file-management-tasks"></a>Tarefas de Gerenciamento de Arquivos

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Tarefas de gerenciamento de arquivo automatizam o processo de descoberta de arquivos em um servidor e aplicam comandos simples. Essas tarefas podem ser agendadas para ocorrer periodicamente e reduzir os custos de repetitivos. Arquivos que serão processados por uma tarefa de gerenciamento de arquivo podem ser definidos por meio de qualquer uma das seguintes propriedades:

-   Local
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

## <a name="see-also"></a>Veja também

-   [Definindo Opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)


