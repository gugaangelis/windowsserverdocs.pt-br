---
title: Sincronizar seu servidor de arquivos com a nuvem usando a sincronização de arquivo do Azure
description: Use sincronização de arquivo do Azure para centralizar compartilhamentos de arquivos da sua organização no Azure, tendo em vista a flexibilidade, desempenho e compatibilidade de um servidor de arquivos local. Sincronização de arquivo Azure transforma Windows Server em um cache rápido do seu compartilhamento de arquivo do Azure com o recurso de nuvem opcional hierárquica.
ms.technology: manage
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cfa469a3c14410d95b0b224cbf59b913ec9f7e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296839"
---
# Sincronizar seu servidor de arquivos com a nuvem usando a sincronização de arquivo do Azure

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Use sincronização de arquivo do Azure para centralizar compartilhamentos de arquivos da sua organização no Azure, tendo em vista a flexibilidade, desempenho e compatibilidade de um servidor de arquivos local. Sincronização de arquivo Azure transforma Windows Server em um cache rápido do seu compartilhamento de arquivo do Azure com o recurso de nuvem opcional hierárquica. Você pode usar qualquer protocolo que está disponível no Windows Server para acessar seus dados localmente, incluindo SMB, NFS e FTPS.

Depois que os arquivos têm sincronizados na nuvem, você pode conectar vários servidores para o mesmo compartilhamento de arquivo do Azure para sincronizar e armazenar em cache o conteúdo localmente — permissões (ACLs) sempre são transportadas também. Arquivos do Azure oferece uma funcionalidade de instantâneo que pode gerar diferenciais instantâneos do seu compartilhamento de arquivo do Azure. Esses instantâneos ainda podem ser montados como unidades de rede somente leitura por meio do SMB para navegação fácil e restauração. Combinado com a nuvem hierarquia, executar um servidor de arquivos local nunca foi tão fácil.

Para obter mais informações, consulte [Planejando uma implantação de sincronização de arquivo do Azure](https://aka.ms/afs).