---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guia de Implantação do AD FS do Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c0f8dca425f644952c36a289ec72651f6383e846
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192191"
---
# <a name="deploying-a-federation-server-farm"></a>Implantando um farm de servidores de federação


Para implantar um farm de servidores de federação, conclua as tarefas desta lista de verificação na ordem. Quando houver um link de referência para um tópico conceitual, retorne a esta lista de verificação depois de analisar o tópico conceitual, para realizar as tarefas restantes da lista de verificação.  
  
![implantação de farm de servidor federado](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: Implantar um Farm de servidores de Federação**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![implantação de farm de servidor federado](media/icon_checkboxo.gif)|Examine os conceitos importantes e considerações enquanto se prepara para implantar serviços de Federação do Active Directory \(do AD FS\). **Observação:**|![implantação de farm de servidor federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[guia de Design do AD FS no Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![implantação de farm de servidor federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Se você decidir usar o Microsoft SQL Server como repositório de configuração do AD FS, certifique-se de implantar uma instância funcional do SQL Server.|[SQL Server](https://technet.microsoft.com/sqlserver) **Warning:** No Windows Server 2012 R2, se você quiser criar um farm do AD FS e usar o SQL Server para armazenar seus dados de configuração, pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012.|  
|![implantação de farm de servidor federado](media/icon_checkboxo.gif)|Una seu computador a um domínio do Active Directory.|![implantação de farm de servidor federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[ingressar um computador em um domínio](Join-a-Computer-to-a-Domain.md)|  
|![implantação de farm de servidor federado](media/icon_checkboxo.gif)|Registrar um Secure Socket Layer \(SSL\) certificado para o AD FS.|![implantação de farm de servidor federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[registrar um certificado SSL para o AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![implantação de farm de servidor federado](media/icon_checkboxo.gif)|Instale o serviço de função do AD FS.|![implantação de farm de servidor federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[instalar o serviço de função do AD FS](Install-the-AD-FS-Role-Service.md)|  
|![implantação de farm de servidor federado](media/icon_checkboxo.gif)|Configure um servidor de federação.|![implantação de farm de servidor federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurar um servidor de Federação](Configure-a-Federation-Server.md)|  
|![implantação de farm de servidor federado](media/icon_checkboxo.gif)|Etapa opcional: Configurar um servidor de federação com o Device Registration Service \(DRS\).|![implantação de farm de servidor federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar um servidor de federação com o Device Registration Service](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![implantação de farm de servidor federado](media/icon_checkboxo.gif)|Adicionar um host \(um\) e o alias \(CNAME\) registro de recurso do sistema de nomes de domínio corporativo \(DNS\) para o serviço de Federação e o DRS.|![implantação de farm de servidor federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar DNS corporativo para o serviço de Federação e o DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![implantação de farm de servidor federado](media/icon_checkboxo.gif)|Verificar se um servidor de federação está operacional.|![implantação de farm de servidor federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Verifique uma federação servidor está operacional](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Consulte também  
[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

