---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: "Guia de implantação do Windows Server 2012 R2 AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f1ea6830237813e6fd2bd6a172f467e8d81065
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-a-federation-server-farm"></a>Implantando um Farm de servidores de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Para implantar um farm de servidores de federação, conclua as tarefas nesta lista de verificação na ordem. Quando um link de referência leva você para um tópico conceitual, retorne a essa lista de verificação depois que você revise o tópico conceitual para que você pode continuar com as tarefas restantes desta lista de verificação.  
  
![Implantando farm de servidores agrupados](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: implantar um Farm de servidores de Federação**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Implantando farm de servidores agrupados](media/icon_checkboxo.gif)|Examine conceitos importantes e as considerações ao se preparar implantar \(AD FS\) serviços de Federação do Active Directory. **Observação:**|![Implantando farm de servidores agrupados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[guia de Design do AD FS no Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![Implantando farm de servidores agrupados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conceitos-chave de Noções básicas sobre AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Se você decidir usar o Microsoft SQL Server para seu repositório de configuração do AD FS, certifique-se para implantar uma instância do SQL Server funcional.|[SQL Server](https://technet.microsoft.com/sqlserver)**Aviso:** no Windows Server 2012 R2, se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012.|  
|![Implantando farm de servidores agrupados](media/icon_checkboxo.gif)|Ingresse o computador em um domínio do Active Directory.|![Implantando farm de servidores agrupados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[ingressar um computador em um domínio](Join-a-Computer-to-a-Domain.md)|  
|![Implantando farm de servidores agrupados](media/icon_checkboxo.gif)|Registre um certificado SSL \(SSL\) do AD FS.|![Implantando farm de servidores agrupados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[registrar um certificado SSL do AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![Implantando farm de servidores agrupados](media/icon_checkboxo.gif)|Instale o serviço de função do AD FS.|![Implantando farm de servidores agrupados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[instalar o serviço de função do AD FS](Install-the-AD-FS-Role-Service.md)|  
|![Implantando farm de servidores agrupados](media/icon_checkboxo.gif)|Configure um servidor de Federação.|![Implantando farm de servidores agrupados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurar um servidor de Federação](Configure-a-Federation-Server.md)|  
|![Implantando farm de servidores agrupados](media/icon_checkboxo.gif)|Etapa opcional: Configure um servidor de federação com o serviço de registro de dispositivo \(DRS\).|![Implantando farm de servidores agrupados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar um servidor de federação com o serviço de registro de dispositivo](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![Implantando farm de servidores agrupados](media/icon_checkboxo.gif)|Adicione um registro host \(A\) e alias \(CNAME\) recursos corporativos \(DNS\) Domain Name System para o serviço de Federação e DRS.|![Implantando farm de servidores agrupados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar corporativa DNS para o serviço de Federação e DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![Implantando farm de servidores agrupados](media/icon_checkboxo.gif)|Verifique se que um servidor de Federação está funcionando.|![Implantando farm de servidores agrupados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[verificar se uma federação servidor é operacional](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Consulte também  
[AD FS implantação](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

