---
title: Migrar o agente de web do AD FS
description: "Fornece informações sobre o agente de web do AD FS para o Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 945a5f4cf0e6c491479b095671ff5e77416c6fa3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrar o agente de web do AD FS

Para migrar o AD FS 1.1 agente baseado em token do Windows ou o AD FS 1.1 agente compatível com declarações que é instalada com o Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012, realizar uma atualização in-loco do sistema operacional do computador que hospeda o agente para o Windows Server 2012. Para obter mais informações, consulte [instalando o Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Nenhuma configuração adicional é necessária.  
  
> [!IMPORTANT]
>  O migrados AD FS 1.1 Windows agente baseado em token só funciona com um serviço de Federação 1.1 AD FS instalada com o Windows Server 2008 R2 ou Windows Server 2008. Para obter mais informações, consulte [Interoperando com o AD FS 1. x](Interoperating-with-AD-FS-1.x.md).  
>   
>  O migrados AD FS 1.1 web com reconhecimento de declarações do agente funciona com o seguinte:  
>   
>  -   Serviço de Federação do AD FS 1.1 instalado com o Windows Server 2008 R2 ou Windows Server 2008  
> -   Serviço de Federação do AD FS 2.0 instalado no Windows Server 2008 R2 ou Windows Server 2008  
> -   Serviço de Federação do AD FS instalada com o Windows Server 2012  
>   
>  Para obter mais informações, consulte [Interoperando com o AD FS 1. x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)