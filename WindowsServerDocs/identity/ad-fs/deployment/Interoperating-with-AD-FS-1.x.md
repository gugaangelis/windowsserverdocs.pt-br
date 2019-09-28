---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Implantando servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f2aaca5ffc846c41af82c276750c564db38b5020
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359514"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperando com o AD FS 1.x

Para interoperabilidade entre Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 no Windows Server® 2012 e AD FS 1. *x*, conclua uma ou mais das seguintes tarefas, dependendo das necessidades da sua organização:  
  
-   Planeje a interoperabilidade entre AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba mais sobre o tipo de declaração ID de nome. Para obter mais informações, consulte [planejando a interoperabilidade com o AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Se você estiver enviando declarações de um AD FS Serviço de Federação no Windows Server 2012 que pode ser consumido por um AD FS 1. *x* serviço de Federação, consulte [Checklist: Configurar AD FS para enviar declarações para um AD FS 1. x Serviço de Federação @ no__t-0.  
  
-   Se você estiver enviando declarações de um AD FS Serviço de Federação no Windows Server 2012 que pode ser consumido por um aplicativo que é hospedado por um servidor Web que executa o AD FS 1. *x* Claims @ no__t-1aware Web Agent, consulte [Checklist: Configurar AD FS para enviar declarações para um agente Web com reconhecimento de declarações do AD FS 1. x @ no__t-0.  
  
-   Se você estiver enviando declarações de um AD FS 1. *x* serviço de Federação ser consumido por um AD FS serviço de Federação no Windows Server 2012, consulte [Checklist: Configurando AD FS para consumir declarações de AD FS 1. x @ no__t-0.  
  
## <a name="differences-between-federation-service-settings"></a>Diferenças entre configurações de Serviço de Federação  
Embora a maioria das AD FS 1. as configurações *x* serviço de Federação funcionam de maneira semelhante ao AD FS serviço de Federação nas configurações do Windows Server 2012, alguns nomes de configuração foram alterados. A tabela a seguir lista os nomes de configurações para um AD FS 1. *x* serviço de Federação e seus nomes equivalentes para um AD FS serviço de Federação no Windows Server 2012.  
  
|Configuração de Serviço de Federação AD FS 1. x|AD FS equivalente Serviço de Federação na configuração do Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Parceiro de conta|Relação de confiança do provedor de declarações  
|Parceiro de recurso|Objeto de confiança de terceira parte confiável 
|Aplicativo|Objeto de confiança de terceira parte confiável  
|Propriedades do Aplicativo|Propriedades de confiança de terceira parte confiável  
|URL do aplicativo|Identificador de terceira parte confiável e URL de ponto de extremidade passivo WS @ no__t-0Federation  
|URI de Serviço de Federação|Identificador do Serviço de Federação  
|URL do ponto de extremidade Serviço de Federação|URL de ponto de extremidade passivo WS @ no__t-0Federation  
  
## <a name="see-also"></a>Consulte também  
[Interoperabilidade entre AD FS e AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

