---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Implantando servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0eb9265513d5ca18da1150d3be6752d364b7cd1a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192085"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperando com o AD FS 1.x

Para interoperabilidade entre serviços de Federação do Active Directory \(do AD FS\) no Windows Server® 2012 e do AD FS 1. *x*, execute uma ou mais das tarefas a seguir, dependendo das necessidades da sua organização:  
  
-   Planejar a interoperabilidade entre o AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba que mais sobre a ID de nome de tipo de declaração. Para obter mais informações, consulte [planejamento para interoperabilidade com o AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Se você estiver enviando declarações de um serviço de Federação do AD FS no Windows Server 2012 que podem ser consumidos por um AD FS 1. *x* serviço de federação, consulte [lista de verificação: Configurar o AD FS para enviar as declarações para um serviço de Federação do AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Se você estiver enviando declarações de um serviço de Federação do AD FS no Windows Server 2012 que podem ser consumidos por um aplicativo que é hospedado por um servidor Web executando o AD FS 1. *x* declarações\-Agente Web com reconhecimento, consulte [lista de verificação: Configurar o AD FS para enviar as declarações para um agente Web com reconhecimento de declarações do AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Se você estiver enviando declarações de um AD FS 1. *x* serviço de Federação a ser consumido por um serviço de Federação do AD FS no Windows Server 2012, consulte [lista de verificação: Configurar o AD FS para consumir as declarações do AD FS 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Diferenças entre as configurações do serviço de Federação  
Embora a maioria dos AD FS 1. *x* trabalho de configurações do serviço de federação de maneira semelhante, como o serviço de Federação do AD FS nas configurações do Windows Server 2012, alguns nomes de configuração foram alterados. A tabela a seguir lista os nomes das configurações de um AD FS 1. *x* serviço de Federação e seus nomes equivalentes para um serviço de Federação do AD FS no Windows Server 2012.  
  
|Configuração de serviço de Federação do AD FS 1.x|Serviço de Federação equivalente do AD FS na configuração do Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Parceiro de conta|Relação de confiança do provedor de declarações  
|Parceiro de recurso|Objeto de confiança de terceira parte confiável 
|Aplicativo|Objeto de confiança de terceira parte confiável  
|Propriedades do Aplicativo|As propriedades da entidade de confiança de terceira parte confiável  
|URL do aplicativo|Identificador de terceira parte confiável e WS\-URL de ponto de extremidade passivo da federação  
|URI do serviço de Federação|Identificador do Serviço de Federação  
|URL do ponto de extremidade de serviço de Federação|WS\-URL de ponto de extremidade passivo da federação  
  
## <a name="see-also"></a>Consulte também  
[O AD FS e interoperabilidade do AD FS 1.x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

