---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planejamento para interoperabilidade com o AD FS 1.x
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e9f72bd83c90a804749329521a72e3232589c735
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407962"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planejamento para interoperabilidade com o AD FS 1.x

Serviços de Federação do Active Directory (AD FS) os servidores de Federação \(AD FS @ no__t-1 que executam o Windows Server® 2012 podem interoperar com um AD FS 1,0 \(installed com o Windows Server 2003 R2 @ no__t-3 Serviço de Federação e um AD FS 1,1 \(installed com Windows Server 2008 ou Windows Server 2008 R2 @ no__t-5 Serviço de Federação. Qualquer uma das seguintes combinações de interoperabilidade tem suporte:  

-   Qualquer AD FS 1. *x* serviço de Federação pode enviar uma declaração que pode ser consumida por um AD FS serviço de Federação no Windows Server 2012. Para obter mais informações, consulte [Checklist: Configurando AD FS para consumir declarações de AD FS 1. x @ no__t-0.  

-   Qualquer AD FS Serviço de Federação no Windows Server 2012 pode enviar um AD FS 1. Declaração *x*\-compatible que pode ser consumida por um AD FS 1. *x* serviço de Federação. Para obter mais informações, consulte [Checklist: Configurar AD FS para enviar declarações para um AD FS 1. x Serviço de Federação @ no__t-0.  

-   Qualquer AD FS Serviço de Federação no Windows Server 2012 pode enviar um AD FS 1. a Declaração *x*\-compatible que pode ser consumida por um ou mais servidores Web que executam o AD FS 1. *x* Claims @ no__t-3aware Web Agent. Para obter mais informações, consulte [Checklist: Configurar AD FS para enviar declarações para um agente Web com reconhecimento de declarações do AD FS 1. x @ no__t-0.  

> [!NOTE]  
> AD FS não dá suporte ou interoperabilidade com o AD FS 1. Agente Web baseado no token *x* do Windows NT.  

Um AD FS 1. a Declaração *x*\-compatible é uma declaração que pode ser enviada por uma serviço de Federação AD FS no Windows Server 2012 e compreendida por um AD FS 1. *x* serviço de Federação. Para que um AD FS 1. *x* serviço de Federação pode consumir as declarações que um AD FS serviço de Federação envia, um tipo de declaração de identificador de nome \(ID @ no__t-2 deve ser enviado.  

## <a name="understanding-the-name-id-claim-type"></a>Entendendo o tipo de declaração de ID de Nome  
O tipo de declaração de ID de Nome é o equivalente do tipo de declaração de identidade que o AD FS 1.*x* usa. Ele deve ser usado sempre que quiser para interoperar com o AD FS 1.*x*. O tipo de declaração ID de nome permite um AD FS 1. *x* serviço de Federação ou o AD FS 1. *x* Claims @ no__t-2aware Web Agent para consumir declarações que AD FS no Windows Server 2012 envia, desde que essas declarações sejam enviadas em um dos formatos de ID de nome na tabela a seguir.  


|      Formato da ID de Nome       |               URI correspondente                |
|---------------------------|------------------------------------------------|
| Endereço de email do AD FS 1.*x* | http://schemas.xmlsoap.org/claims/EmailAddress |
|   UPN de email do AD FS 1.*x*   |     http://schemas.xmlsoap.org/claims/UPN      |
|        Nome comum        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           Grupo           |    http://schemas.xmlsoap.org/claims/Group     |

Somente uma declaração de ID de Nome no formato apropriado deve ser enviada. Quando esse critério for atendido, muitas outras declarações podem ser enviadas também, supondo que estão em conformidade com as restrições descritas na tabela.  

> [!NOTE]  
> Um AD FS 1. *x* serviço de Federação pode interpretar somente os tipos de declaração de entrada que começam com o Uniform Resource Identifier \(URI @ no__t-2 de http://schemas.xmlsoap.org/claims/.  

## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
