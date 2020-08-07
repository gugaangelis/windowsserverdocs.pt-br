---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Interoperando com o AD FS 1.x
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 21f0ef0e3ad77d5ea58e5d3b82da66ce420e5d49
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972143"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperando com o AD FS 1.x

Para interoperabilidade entre Serviços de Federação do Active Directory (AD FS) \( AD FS \) no Windows Server &reg; 2012 e no AD FS 1.* x*, conclua uma ou mais das seguintes tarefas, dependendo das necessidades da sua organização:

-   Planeje a interoperabilidade entre AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba mais sobre o tipo de declaração ID de nome. Para obter mais informações, consulte [planejando a interoperabilidade com o AD FS 1. x](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11)).

-   Se você estiver enviando declarações de um AD FS Serviço de Federação no Windows Server 2012 que pode ser consumido por um AD FS 1. *x* serviço de Federação, consulte [lista de verificação: configurando AD FS para enviar declarações para um serviço de Federação de AD FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).

-   Se você estiver enviando declarações de um AD FS Serviço de Federação no Windows Server 2012 que pode ser consumido por um aplicativo que é hospedado por um servidor Web que executa o AD FS 1. *x* \- Agente Web com reconhecimento de declarações, consulte [lista de verificação: Configurar AD FS para enviar declarações para um agente Web com reconhecimento de declarações do AD FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).

-   Se você estiver enviando declarações de um AD FS 1. *x* serviço de Federação ser consumido por um AD FS serviço de Federação no Windows Server 2012, consulte [lista de verificação: configurando AD FS para consumir declarações do AD FS 1. x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).

## <a name="differences-between-federation-service-settings"></a>Diferenças entre configurações de Serviço de Federação
Embora a maioria das AD FS 1. as configurações *x* serviço de Federação funcionam de maneira semelhante ao AD FS serviço de Federação nas configurações do Windows Server 2012, alguns nomes de configuração foram alterados. A tabela a seguir lista os nomes de configurações para um AD FS 1. *x* serviço de Federação e seus nomes equivalentes para um AD FS serviço de Federação no Windows Server 2012.

|Configuração de Serviço de Federação AD FS 1. x|AD FS equivalente Serviço de Federação na configuração do Windows Server 2012
|----------------------------------------|----------------------------------------------------------------------------------------------------------
|Parceiro de conta|Relação de confiança do provedor de declarações
|Parceiro de recurso|Objeto de confiança de terceira parte confiável
|Aplicativo|Objeto de confiança de terceira parte confiável
|Propriedades do aplicativo|Propriedades de confiança de terceira parte confiável
|URL do Aplicativo|Identificador de terceira parte confiável e \- URL de ponto de extremidade passivo do WS Federation
|URI de Serviço de Federação|Identificador do Serviço de Federação
|URL do ponto de extremidade Serviço de Federação|\-URL de ponto de extremidade passivo do WS Federation

## <a name="see-also"></a>Consulte Também
[Interoperabilidade entre AD FS e AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)

