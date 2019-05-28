---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planejamento para interoperabilidade com o AD FS 1.x
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7a1082b873f65a9f98b25425a392b2c62de8ca22
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191006"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planejamento para interoperabilidade com o AD FS 1.x

Serviços de Federação do Active Directory \(do AD FS\) servidores de federação que executam o Windows Server® 2012 podem interoperar com ambas as an AD FS 1.0 \(instalado com o Windows Server 2003 R2\) federação e um serviço do AD FS 1.1 \(instalado com o Windows Server 2008 ou Windows Server 2008 R2\) serviço de Federação. Qualquer uma das seguintes combinações de interoperabilidade tem suporte:  
  
-   Qualquer do AD FS 1. *x* serviço de Federação pode enviar uma declaração que pode ser consumida por um serviço de Federação do AD FS no Windows Server 2012. Para obter mais informações, consulte [lista de verificação: Configurar o AD FS para consumir as declarações do AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
-   Qualquer serviço de Federação do AD FS no Windows Server 2012 pode enviar um AD FS 1. *x*\-declaração compatível que pode ser consumida por um AD FS 1. *x* serviço de Federação. Para obter mais informações, consulte [lista de verificação: Configurar o AD FS para enviar as declarações para um serviço de Federação do AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Qualquer serviço de Federação do AD FS no Windows Server 2012 pode enviar um AD FS 1. *x*\-declaração compatível que pode ser consumida por um ou mais servidores Web em execução do AD FS 1. *x* declarações\-Agente Web com reconhecimento. Para obter mais informações, consulte [lista de verificação: Configurar o AD FS para enviar as declarações para um agente Web com reconhecimento de declarações do AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
> [!NOTE]  
> O AD FS não oferece suporte ou interopera com o AD FS 1. *x* Agente Web baseado em token do Windows NT.  
  
Um AD FS 1. *x*\-declaração compatível com é uma declaração que pode ser enviada por um serviço de Federação do AD FS no Windows Server 2012 e entendida por um AD FS 1. *x* serviço de Federação. Para que um AD FS 1. *x* serviço de Federação possa consumir as declarações que um serviço de Federação do AD FS envia, um identificador de nome \(ID\) de declaração de tipo deve ser enviado.  
  
## <a name="understanding-the-nameid-claim-type"></a>Entendendo o tipo de declaração de ID de Nome  
O tipo de declaração de ID de Nome é o equivalente do tipo de declaração de identidade que o AD FS 1.*x* usa. Ele deve ser usado sempre que quiser para interoperar com o AD FS 1.*x*. A declaração de ID de nome de tipo permite que seja um AD FS 1. *x* serviço de Federação ou do AD FS 1. *x* declarações\-Agente Web com reconhecimento de consumam as declarações que envia o AD FS no Windows Server 2012, desde que essas declarações sejam enviadas em um dos formatos de ID de nome na tabela a seguir.  
  
|Formato da ID de Nome|URI correspondente|  
|------------------|---------------------|  
|Endereço de email do AD FS 1.*x*|http://schemas.xmlsoap.org/claims/EmailAddress|  
|UPN de email do AD FS 1.*x*|http://schemas.xmlsoap.org/claims/UPN|  
|Nome comum|http://schemas.xmlsoap.org/claims/CommonName|  
|Grupo|http://schemas.xmlsoap.org/claims/Group|  
  
Somente uma declaração de ID de Nome no formato apropriado deve ser enviada. Quando esse critério for atendido, muitas outras declarações podem ser enviadas também, supondo que estão em conformidade com as restrições descritas na tabela.  
  
> [!NOTE]  
> Um AD FS 1. *x* serviço de Federação pode interpretar a entrada apenas tipos de declaração que começam com o Uniform Resource Identifier \(URI\) de http://schemas.xmlsoap.org/claims/.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
