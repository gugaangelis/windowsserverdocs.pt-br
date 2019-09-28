---
title: 'Lista de verificação: implantar namespaces DFS'
description: Este artigo descreve como configurar e implantar namespaces DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ab38c41c32ec88285a69fb94e62abc1453ddc3d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402270"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Lista de verificação: Implantar namespaces DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Os namespaces do Sistema de Arquivos Distribuído (DFS) e Replicação do DFS podem ser usados para publicar documentos, software e dados de linha de negócios para usuários em toda a organização. Embora Replicação do DFS sozinho seja suficiente para distribuir dados, você pode usar namespaces do DFS para configurar o namespace de forma que uma pasta seja hospedada por vários servidores, cada uma delas contendo uma cópia atualizada da pasta. Isso aumenta a disponibilidade de dados e distribui a carga de cliente em todos os servidores.

Quando uma pasta no namespace de navegação, os usuários não são cientes de que a pasta é hospedada por vários servidores. Quando um usuário abre a pasta, o computador cliente é chamado automaticamente em um servidor em seu site. Se nenhum servidor de mesmo site estiver disponível, você poderá configurar o namespace para referenciar o cliente a um servidor que tenha o menor custo de conexão, conforme definido nos serviços de diretório Active Directory (AD DS).

Para implantar namespaces DFS, realize as seguintes tarefas:

-   Revise os conceitos básicos e requisitos de Namespaces DFS.
[Visão geral dos namespaces do DFS](dfs-overview.md)
-   [Escolher um tipo de namespace](choose-a-namespace-type.md)
-   [Criar um namespace do DFS](create-a-dfs-namespace.md) 
-   Migre namespaces existentes baseados em domínio para namespaces baseados em domínio do Windows Server 2008 modo. [Migrar um namespace baseado em domínio para o modo do Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Aumente a disponibilidade adicionando servidores de namespace em um namespace baseado em domínio. [Adicionar servidores de namespace a um namespace do DFS baseado em domínio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Adicionar pastas a um namespace. [Criar uma pasta em um namespace do DFS](create-a-folder-in-a-dfs-namespace.md)
-   Adicione destinos de pasta pastas em um namespace. [Adicionar destinos de pastas](add-folder-targets.md)
-   Replica o conteúdo entre os destinos de pasta usando replicação do DFS (opcional). [Replicar destinos de pasta usando Replicação do DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Consulte também

-   [Namespaces](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de verificação: ajustar um Namespace do DFS](checklist-tune-a-dfs-namespace.md)
-   [Replicação](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


