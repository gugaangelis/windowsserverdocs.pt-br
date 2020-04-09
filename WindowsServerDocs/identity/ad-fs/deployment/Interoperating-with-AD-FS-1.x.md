---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Implantando servidores de federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 800c7fc23c9b126a17e54311fc6df0d3dcf36b4c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855359"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperando com o AD FS 1.x

Para interoperabilidade entre Serviços de Federação do Active Directory (AD FS) \(AD FS\) no Windows Server&reg; 2012 e no AD FS 1. *x*, conclua uma ou mais das seguintes tarefas, dependendo das necessidades da sua organização:  
  
-   Planeje a interoperabilidade entre AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba mais sobre o tipo de declaração ID de nome. Para obter mais informações, consulte [planejando a interoperabilidade com o AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Se você estiver enviando declarações de um AD FS Serviço de Federação no Windows Server 2012 que pode ser consumido por um AD FS 1. *x* serviço de Federação, consulte [lista de verificação: configurando AD FS para enviar declarações para um serviço de Federação de AD FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Se você estiver enviando declarações de um AD FS Serviço de Federação no Windows Server 2012 que pode ser consumido por um aplicativo que é hospedado por um servidor Web que executa o AD FS 1. *x* alega\-Agente Web ciente, consulte [lista de verificação: configurando AD FS para enviar declarações para um agente Web com reconhecimento de declaração AD FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Se você estiver enviando declarações de um AD FS 1. *x* serviço de Federação ser consumido por um AD FS serviço de Federação no Windows Server 2012, consulte [lista de verificação: configurando AD FS para consumir declarações do AD FS 1. x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Diferenças entre configurações de Serviço de Federação  
Embora a maioria das AD FS 1. as configurações *x* serviço de Federação funcionam de maneira semelhante ao AD FS serviço de Federação nas configurações do Windows Server 2012, alguns nomes de configuração foram alterados. A tabela a seguir lista os nomes de configurações para um AD FS 1. *x* serviço de Federação e seus nomes equivalentes para um AD FS serviço de Federação no Windows Server 2012.  
  
|Configuração de Serviço de Federação AD FS 1. x|AD FS equivalente Serviço de Federação na configuração do Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Parceiro de conta|Relação de confiança do provedor de declarações  
|Parceiro de recurso|Parte confiável subjacente 
|Aplicativo|Parte confiável subjacente  
|Propriedades do aplicativo|Propriedades de confiança de terceira parte confiável  
|URL do aplicativo|Identificador de terceira parte confiável e URL de ponto de extremidade passivo da Federação WS\-  
|URI de Serviço de Federação|Identificador do Serviço de Federação  
|URL do ponto de extremidade Serviço de Federação|URL de ponto de extremidade passivo de Federação do WS\-  
  
## <a name="see-also"></a>Consulte também  
[Interoperabilidade entre AD FS e AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

