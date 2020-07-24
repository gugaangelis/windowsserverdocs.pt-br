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
ms.openlocfilehash: 389d02ae7269ef48ef77d066db3064759d9253c1
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961958"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Lista de verificação: implantar namespaces DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Namespaces de DFS (File System) distribuídos e replicação de DFS podem ser usados para publicar documentos, software e dados de linha de negócios para usuários em toda a organização. Embora a replicação DFS sozinha seja suficiente para distribuir os dados, você pode usar os Namespaces de DFS para configurar o namespace para que uma pasta hospedada por vários servidores, cada um deles contém uma cópia atualizada da pasta. Isso aumenta a disponibilidade de dados e distribui a carga de cliente em todos os servidores.

Quando uma pasta no namespace de navegação, os usuários não são cientes de que a pasta é hospedada por vários servidores. Quando um usuário abre a pasta, o computador cliente é chamado automaticamente em um servidor em seu site. Se não houver servidores de mesmo site disponíveis, você pode configurar o namespace para referir-se o cliente para um servidor que tem a conexão de menor custo conforme definido nos serviços de diretório do Active Directory (AD DS).

Para implantar namespaces DFS, realize as seguintes tarefas:

-   Revise os conceitos básicos e requisitos de Namespaces DFS.
[Visão geral de Namespaces DFS](dfs-overview.md)
-   [Escolher um tipo de namespace](choose-a-namespace-type.md)
-   [Criar um namespace do DFS](create-a-dfs-namespace.md)
-   Migre namespaces existentes baseados em domínio para namespaces baseados em domínio do Windows Server 2008 modo. [Migrar um namespace baseado em domínio para o modo do Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)
-   Aumente a disponibilidade adicionando servidores de namespace em um namespace baseado em domínio. [Adicionar servidores de namespace a um namespace do DFS baseado em domínio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Adicionar pastas a um namespace. [Criar uma pasta em um namespace do DFS](create-a-folder-in-a-dfs-namespace.md)
-   Adicione destinos de pasta pastas em um namespace. [Adicionar destinos de pastas](add-folder-targets.md)
-   Replica o conteúdo entre os destinos de pasta usando replicação do DFS (opcional). [Replicar destinos de pasta usando Replicação do DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="additional-references"></a>Referências adicionais

-   [Namespaces](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771914(v=ws.11))
-   [Lista de verificação: ajustar um Namespace do DFS](checklist-tune-a-dfs-namespace.md)
-   [Replicação](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770278(v=ws.11))
