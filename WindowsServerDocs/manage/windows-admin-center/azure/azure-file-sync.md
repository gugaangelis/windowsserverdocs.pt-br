---
title: Sincronizar seu servidor de arquivos com a nuvem usando a Sincronização de Arquivos do Azure
description: Use Sincronização de Arquivos do Azure para centralizar os compartilhamentos de arquivos da sua organização no Azure, mantendo, ao mesmo tempo, a flexibilidade, o desempenho e a compatibilidade de um servidor de arquivos local. Sincronização de Arquivos do Azure transforma o Windows Server em um cache rápido do compartilhamento de arquivos do Azure com o recurso de camada de nuvem opcional.
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.openlocfilehash: 56937ad0351a1421ab64b93351d7fa5f6d9f4fe8
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766679"
---
# <a name="sync-your-file-server-with-the-cloud-by-using-azure-file-sync"></a>Sincronizar seu servidor de arquivos com a nuvem usando a Sincronização de Arquivos do Azure

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Use Sincronização de Arquivos do Azure para centralizar os compartilhamentos de arquivos da sua organização no Azure, mantendo, ao mesmo tempo, a flexibilidade, o desempenho e a compatibilidade de um servidor de arquivos local. Sincronização de Arquivos do Azure transforma o Windows Server em um cache rápido do compartilhamento de arquivos do Azure com o recurso de camada de nuvem opcional. Use qualquer protocolo disponível no Windows Server para acessar seus dados localmente, incluindo SMB, NFS e FTPS.

Depois que os arquivos forem sincronizados com a nuvem, você poderá conectar vários servidores ao mesmo compartilhamento de arquivos do Azure para sincronizar e armazenar o conteúdo em cache localmente — as permissões (ACLs) sempre serão transportadas. O arquivos do Azure oferece um recurso de instantâneo que pode gerar instantâneos diferenciais de seu compartilhamento de arquivos do Azure. Esses instantâneos podem até mesmo ser montados como unidades de rede somente leitura via SMB para facilitar a navegação e a restauração. Combinado com camadas de nuvem, a execução de um servidor de arquivos local nunca foi tão fácil.

Para obter mais informações, consulte [planejando uma implantação de sincronização de arquivos do Azure](/azure/storage/files/storage-sync-files-planning).