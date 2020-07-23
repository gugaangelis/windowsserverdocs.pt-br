---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guia de Implantação do AD FS do Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 70818fffb601d7da6b9e7e6f8a1e2a774abf2ca5
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965478"
---
# <a name="deploying-a-federation-server-farm"></a>Implantação de um farm de servidores de federação


Para implantar um farm de servidores de federação, conclua as tarefas desta lista de verificação na ordem. Quando um link de referência levá-lo para um tópico conceitual, retorne a esta lista de verificação depois de revisar o tópico conceitual para que você possa prosseguir com as tarefas restantes desta lista de verificação.  
  
![](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação de implantação do farm de servidores federados: Implantando um farm de servidores de Federação**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Implantando farm de servidores federados](media/icon_checkboxo.gif)|Examine os conceitos e considerações importantes ao se preparar para implantar Serviços de Federação do Active Directory (AD FS) \( AD FS \) . **Observação**:|![Implantando o](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Guia de design do farm de servidores federados AD FS no Windows server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<p>![Implantando o farm de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[entendendo os principais conceitos de AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Se você decidir usar o Microsoft SQL Server como repositório de configuração do AD FS, certifique-se de implantar uma instância funcional do SQL Server.|Aviso de [SQL Server](/sql/sql-server/?view=sql-server-ver15) **:** no Windows Server 2012 R2, se você quiser criar um farm de AD FS e usar SQL Server para armazenar seus dados de configuração, poderá usar SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012.|  
|![Implantando farm de servidores federados](media/icon_checkboxo.gif)|Una seu computador a um domínio do Active Directory.|![Implantando o farm de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[ingressar um computador em um domínio](Join-a-Computer-to-a-Domain.md)|  
|![Implantando farm de servidores federados](media/icon_checkboxo.gif)|Registre um \( certificado SSL de camada \) de soquete seguro para AD FS.|![Implantando o farm](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[de servidores federados registrar um certificado SSL para AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![Implantando farm de servidores federados](media/icon_checkboxo.gif)|Instale o serviço de função do AD FS.|![Implantando o farm de servidores federados,](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[Instale o serviço de função AD FS](Install-the-AD-FS-Role-Service.md)|  
|![Implantando farm de servidores federados](media/icon_checkboxo.gif)|Configure um servidor de federação.|![Implantando o farm](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[de servidores federados configurar um servidor de Federação](Configure-a-Federation-Server.md)|  
|![Implantando farm de servidores federados](media/icon_checkboxo.gif)|Etapa opcional: configurar um servidor de Federação com o serviço de registro de dispositivo \( DRS \) .|![Implantando o farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de servidores federados configurar um servidor de Federação com o serviço de registro de dispositivo](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![Implantando farm de servidores federados](media/icon_checkboxo.gif)|Adicione um \( registro de \) recurso de CNAME a e alias \( \) ao DNS do sistema de nomes de domínio corporativo \( \) para o serviço de Federação e o DRS.|![Implantando o farm de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Configurar o DNS corporativo para o serviço de Federação e o DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![Implantando farm de servidores federados](media/icon_checkboxo.gif)|Verificar se um servidor de federação está operacional.|![Implantando o farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de servidores federados Verifique se um servidor de Federação está operacional](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Consulte Também  
[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de Implantação do AD FS do Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  
