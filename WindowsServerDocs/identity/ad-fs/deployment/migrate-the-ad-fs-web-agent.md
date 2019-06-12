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
ms.openlocfilehash: 7cde62cb23c69a425522e40ed65ee2d40ef28268
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445605"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrar o agente web do AD FS

Para migrar o AD FS 1.1 agente baseado em token do Windows ou o AD FS 1.1 o agente com reconhecimento de declarações que é instalado com o Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012, executar uma atualização in-loco do sistema operacional do computador que hospeda qualquer um dos agentes para o Windows Server 2012. Para obter mais informações, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Nenhuma outra configuração é necessária.  
  
> [!IMPORTANT]
>  O agente baseado em token do Windows no AD FS 1.1 só funciona um serviço de federação do AD FS 1.1 instalado no Windows Server 2008 R2 ou Windows Server 2008. Para obter mais informações, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
> 
>  O agente Web com reconhecimento de declarações do AD FS 1.1 só funciona com o seguinte:  
> 
> - Serviço de federação do AD FS 1.1 instalado com Windows Server 2008 R2 ou Windows Server 2008  
>   -   Serviço de federação do AD FS 2.0 instalado com Windows Server 2008 R2 ou Windows Server 2008  
>   -   Serviço de Federação do AD FS instalado com o Windows Server 2012  
> 
>   Para obter mais informações, consulte [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor do AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy do AD FS 2.0 Federation Server](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor do AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy do AD FS 2.0 Federation Server](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)