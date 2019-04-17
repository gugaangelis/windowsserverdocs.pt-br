---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: "Implantando servidores de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f80425b6f062040c51357353038fd07ff0a79ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperando com o AD FS 1. x

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para fins de interoperabilidade entre \(AD FS\) serviços de Federação do Active Directory no Windows Server® 2012 e do AD FS 1. *x*, execute uma ou mais das seguintes tarefas, dependendo das necessidades da sua organização:  
  
-   Planejar a interoperabilidade entre o AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba que mais sobre a ID do nome do tipo de declaração. Para obter mais informações, consulte [planejamento para interoperabilidade com o AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Se você está enviando requerimentos judiciais ou Extrajudiciais de um serviço de Federação do AD FS no Windows Server 2012 pode ser consumido por um AD FS 1. *x* serviço de federação, consulte [lista de verificação: configurar o AD FS declarações enviar para um AD FS 1. x serviço de Federação](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Se você está enviando requerimentos judiciais ou Extrajudiciais de um serviço de Federação do AD FS no Windows Server 2012, o que pode ser consumido por um aplicativo que está hospedado por um servidor Web executando o AD FS 1. *x* claims\ reconhecimento agente de Web, consulte [lista de verificação: configurar o AD FS declarações enviar para um AD FS 1. x agente da Web compatível com declarações](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Se você está enviando requerimentos judiciais ou Extrajudiciais de um AD FS 1. *x* serviço de federação para ser consumido por um serviço de Federação do AD FS no Windows Server 2012, consulte [lista de verificação: configurar o AD FS para consumir declarações do AD FS 1. x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Diferenças entre as configurações de serviço de Federação  
Embora a maioria do AD FS 1. *x* trabalho de configurações de serviço de federação de maneira semelhante do serviço de Federação do AD FS nas configurações do Windows Server 2012, alguns nomes de configuração mudaram. A tabela a seguir lista os nomes de configurações para um AD FS 1. *x* seus nomes equivalente para um serviço de Federação do AD FS no Windows Server 2012 e do serviço de Federação.  
  
|Configuração de serviço de Federação do AD FS 1. x|Serviço de Federação equivalente do AD FS na configuração do Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Parceiro de conta|Requerimentos judiciais ou Extrajudiciais provedor confiança  
|Parceiro de recurso|Dependência de confiança de terceiros 
|Aplicativo|Dependência de confiança de terceiros  
|Propriedades do aplicativo|Dependência de propriedades de confiança de terceiros  
|URL do aplicativo|Terceira identificador de terceiros e a URL de ponto de extremidade passivo WS\ federação  
|Serviço de Federação URI|Identificador de serviço de Federação  
|URL de ponto de extremidade de serviço de Federação|URL de ponto de extremidade passivo WS\ federação  
  
## <a name="see-also"></a>Consulte também  
[AD FS e interoperabilidade do AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

