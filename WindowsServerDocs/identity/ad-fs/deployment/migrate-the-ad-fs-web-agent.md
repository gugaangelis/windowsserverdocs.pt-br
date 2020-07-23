---
title: Migrar o Agente Web AD FS
description: Fornece informações sobre o Agente Web do AD FS para o Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3c18e0a6a2e633c0fad0ce8585296afba8ce444c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966928"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrar o Agente Web AD FS

Para migrar o agente baseado em token do AD FS 1,1 do Windows ou o agente de reconhecimento de declaração do AD FS 1,1 instalado com o Windows Server 2008 R2 ou o Windows Server 2008 para o Windows Server 2012, execute uma atualização in-loco do sistema operacional do computador que hospeda o agente para o Windows Server 2012. Para obter mais informações, consulte [Instalando o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11)). Nenhuma configuração adicional é necessária.  
  
> [!IMPORTANT]
>  O agente baseado em token do Windows no AD FS 1.1 só funciona um serviço de federação do AD FS 1.1 instalado no Windows Server 2008 R2 ou Windows Server 2008. Para obter mais informações, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
> 
>  O agente Web com reconhecimento de declarações do AD FS 1.1 só funciona com o seguinte:  
> 
> - Serviço de federação do AD FS 1.1 instalado com Windows Server 2008 R2 ou Windows Server 2008  
>   -   Serviço de federação do AD FS 2.0 instalado com Windows Server 2008 R2 ou Windows Server 2008  
>   -   AD FS serviço de Federação instalado com o Windows Server 2012  
> 
>   Para obter mais informações, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o proxy do servidor de Federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar o proxy do servidor de Federação AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)
