---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planejamento para interoperabilidade com o AD FS 1.x
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 94313fc185a4f326ad00a95e4c594fd3e696f61a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967603"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planejamento para interoperabilidade com o AD FS 1.x

Serviços de Federação do Active Directory (AD FS) \( AD FS \) servidores de Federação que executam o windows Server &reg; 2012 podem interoperar com um AD FS 1,0 \( instalado com o windows Server 2003 r2 \) serviço de Federação e um AD FS 1,1 \( instalado com o Windows server 2008 ou o Windows Server 2008 R2 \) serviço de Federação. Qualquer uma das seguintes combinações de interoperabilidade tem suporte:

-   Qualquer AD FS 1. *x* serviço de Federação pode enviar uma declaração que pode ser consumida por um AD FS serviço de Federação no Windows Server 2012. Para obter mais informações, consulte [lista de verificação: Configurando AD FS para consumir declarações do AD FS 1. x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).

-   Qualquer AD FS Serviço de Federação no Windows Server 2012 pode enviar um AD FS 1. *x* \- declaração compatível que pode ser consumida por um AD FS 1. *x* serviço de Federação. Para obter mais informações, consulte [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Federation Service](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).

-   Qualquer AD FS Serviço de Federação no Windows Server 2012 pode enviar um AD FS 1. *x* \- declaração compatível que pode ser consumida por um ou mais servidores Web que executam o AD FS 1. Agente Web com reconhecimento de declarações *x* \- . Para obter mais informações, consulte [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Claims-Aware Web Agent](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).

> [!NOTE]
> AD FS não dá suporte ou interoperabilidade com o AD FS 1. Agente Web baseado no token *x* do Windows NT.

Um AD FS 1. *x* \- a declaração compatível é uma declaração que pode ser enviada por um AD FS Serviço de Federação no Windows Server 2012 e compreendida por um AD FS 1. *x* serviço de Federação. Para que um AD FS 1. *x* serviço de Federação pode consumir as declarações que um AD FS serviço de Federação envia, um tipo de declaração de ID de identificador de nome \( \) deve ser enviado.

## <a name="understanding-the-name-id-claim-type"></a>Entendendo o tipo de declaração de ID de Nome
O tipo de declaração de ID de Nome é o equivalente do tipo de declaração de identidade que o AD FS 1.*x* usa. Ele deve ser usado sempre que quiser para interoperar com o AD FS 1.*x*. O tipo de declaração ID de nome permite um AD FS 1. *x* serviço de Federação ou o AD FS 1. *x* \- o Agente Web com reconhecimento de declarações para consumir declarações que AD FS no Windows Server 2012 envia, desde que essas declarações sejam enviadas em um dos formatos de ID de nome na tabela a seguir.


|      Formato da ID de Nome       |               URI correspondente                |
|---------------------------|------------------------------------------------|
| AD FS 1. Endereço de email *x* | http://schemas.xmlsoap.org/claims/EmailAddress |
|   UPN de email do AD FS 1.*x*   |     http://schemas.xmlsoap.org/claims/UPN      |
|        Nome comum        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           Agrupar           |    http://schemas.xmlsoap.org/claims/Group     |

Somente uma declaração de ID de Nome no formato apropriado deve ser enviada. Quando esse critério for atendido, muitas outras declarações podem ser enviadas também, supondo que estão em conformidade com as restrições descritas na tabela.

> [!NOTE]
> Um AD FS 1. *x* serviço de Federação pode interpretar somente os tipos de declaração de entrada que começam com o \( URI \) de Uniform Resource Identifier de http://schemas.xmlsoap.org/claims/ .

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
