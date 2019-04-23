---
title: Migrar o agente web do AD FS
description: Fornece informações sobre o agente web do AD FS para o Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 945a5f4cf0e6c491479b095671ff5e77416c6fa3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877587"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrar o agente web do AD FS

Para migrar o AD FS 1.1 agente baseado em token do Windows ou o AD FS 1.1 o agente com reconhecimento de declarações que é instalado com o Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012, executar uma atualização in-loco do sistema operacional do computador que hospeda qualquer um dos agentes para o Windows Server 2012. Para obter mais informações, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Nenhuma outra configuração é necessária.  
  
> [!IMPORTANT]
>  O agente baseado em token do Windows no AD FS 1.1 só funciona um serviço de federação do AD FS 1.1 instalado no Windows Server 2008 R2 ou Windows Server 2008. Para obter mais informações, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
>   
>  O agente Web com reconhecimento de declarações do AD FS 1.1 só funciona com o seguinte:  
>   
>  -   Serviço de federação do AD FS 1.1 instalado com Windows Server 2008 R2 ou Windows Server 2008  
> -   Serviço de federação do AD FS 2.0 instalado com Windows Server 2008 R2 ou Windows Server 2008  
> -   Serviço de Federação do AD FS instalado com o Windows Server 2012  
>   
>  Para obter mais informações, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor do AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy do AD FS 2.0 Federation Server](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor do AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy do AD FS 2.0 Federation Server](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os AD FS agentes Web 1.1](migrate-the-ad-fs-web-agent.md)