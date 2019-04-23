---
title: 'Lista de verificação: implantar namespaces DFS'
description: Este artigo descreve como configurar e implantar namespaces DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7f7cca6b67ff6fa8d81e88323381866315f07f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842117"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Lista de verificação: Implante Namespaces DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Namespaces do Distributed File System (DFS) e a replicação DFS podem ser usado para publicar documentos, software e dados de linha de negócios para usuários em toda a organização. Embora a replicação DFS sozinha é suficiente para distribuir os dados, você pode usar Namespaces do DFS para configurar o namespace para que uma pasta é hospedada por vários servidores, cada qual com uma cópia atualizada da pasta. Isso aumenta a disponibilidade de dados e distribui a carga de cliente em todos os servidores.

Quando uma pasta no namespace de navegação, os usuários não são cientes de que a pasta é hospedada por vários servidores. Quando um usuário abre a pasta, o computador cliente é chamado automaticamente em um servidor em seu site. Se não houver servidores do mesmo site disponíveis, você pode configurar o namespace para referir-se o cliente a um servidor que tem a conexão de menor custo, conforme definido nos serviços de diretório do Active Directory (AD DS).

Para implantar namespaces DFS, realize as seguintes tarefas:

-   Revise os conceitos básicos e requisitos de Namespaces DFS.
[Visão geral dos Namespaces DFS](dfs-overview.md)
-   [Escolha um tipo de namespace](choose-a-namespace-type.md)
-   [Criar um namespace do DFS](create-a-dfs-namespace.md) 
-   Migre namespaces existentes baseados em domínio para namespaces baseados em domínio do Windows Server 2008 modo. [Migrar um Namespace baseado em domínio para o modo do Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Aumente a disponibilidade adicionando servidores de namespace em um namespace baseado em domínio. [Adicionar servidores de Namespace para um Namespace do DFS baseado em domínio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Adicionar pastas a um namespace. [Crie uma pasta em um Namespace do DFS](create-a-folder-in-a-dfs-namespace.md)
-   Adicione destinos de pasta pastas em um namespace. [Adicionar destinos de pasta](add-folder-targets.md)
-   Replica o conteúdo entre os destinos de pasta usando replicação do DFS (opcional). [Replicar destinos de pasta usando a replicação do DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Consulte também

-   [Namespaces](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de verificação: Ajustar um Namespace do DFS](checklist-tune-a-dfs-namespace.md)
-   [Replicação](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


