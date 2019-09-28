---
title: Gerenciamento de cota
description: Este artigo descreve como criar e gerenciar cotas
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5a655e28020d08bb1c10fa862c007f914a8cf566
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403077"
---
# <a name="quota-management"></a>Gerenciamento de cota

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

No nó **Gerenciamento de cota** do snap-in do Console de Gerenciamento Microsoft<sup>®</sup> do Gerenciador de Recursos de Servidor de Arquivos, você pode realizar as seguintes tarefas:

-   Criar cotas para limitar o espaço permitido de um volume ou uma pasta e gerar notificações quando os limites de cota são aproximados ou excedidos.
-   Gerar automaticamente cotas de aplicação automática que se aplicam a todas as subpastas existentes em um volume ou uma pasta e todas as subpastas criadas no futuro.
-   Definir modelos de cota que podem ser facilmente aplicados a novos volumes ou pastas e então usá-los em uma organização.

Por exemplo, você pode:

-   Coloque um limite de 200 megabytes (MB) nas pastas de servidor pessoais dos usuários, com uma notificação por email enviada a você e o usuário quando 180 MB de armazenamento for excedido.
-   Defina uma cota flexível de 500 MB na pasta compartilhada de um grupo. Quando esse limite de armazenamento é atingido, todos os usuários no grupo são notificados por email de que a cota de armazenamento foi temporariamente estendida para 520 MB para que eles possam excluir arquivos desnecessários e estar em conformidade com a política de cota predefinida de 500 MB.
-   Receber uma notificação quando uma pasta temporária atingir 2 gigabytes (GB) de uso, mas não limitar a cota da pasta porque é necessário para um serviço em execução em seu servidor.

Esta seção inclui os seguintes tópicos:

-   [Criar uma cota](create-quota.md)
-   [Criar uma cota de aplicação automática](create-auto-apply-quota.md)
-   [Criar um modelo de cota](create-quota-template.md)
-   [Editar propriedades de modelo de cota](edit-quota-template-properties.md)
-   [Editar propriedades de cota de aplicação automática](edit-auto-apply-quota-properties.md)

> [!Note]
> Para definir notificações por email e recursos de relatório, você deve configurar primeiro as opções gerais do Gerenciador de recursos do servidor de arquivos.

## <a name="see-also"></a>Consulte também

-   [Definir opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)


