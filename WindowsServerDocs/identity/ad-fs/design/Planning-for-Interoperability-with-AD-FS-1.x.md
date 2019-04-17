---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planejamento de interoperabilidade com o AD FS 1. x
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f287261ce6cb56e40385ef4de922045153819a23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planejamento de interoperabilidade com o AD FS 1. x

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de federação de \(AD FS\) de serviços de Federação do diretório ativos executando o Windows Server® 2012 podem interoperar com ambos os um AD FS 1.0 \ (instalado com o Windows Server 2003 R2\) serviço de Federação e um AD FS 1.1 \ (instalado com o Windows Server 2008 ou Windows Server 2008 R2\) serviço de Federação. Qualquer uma das seguintes combinações de interoperabilidade têm suporte:  
  
-   Qualquer AD FS 1. *x* serviço de Federação pode enviar uma reivindicação que pode ser consumida por um serviço de Federação do AD FS no Windows Server 2012. Para obter mais informações, consulte [lista de verificação: configurar o AD FS para consumir declarações do AD FS 1. x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
-   Qualquer serviço de Federação do AD FS no Windows Server 2012 pode enviar um AD FS 1. *x*\-compatible reivindicação que pode ser consumida por um AD FS 1. *x* serviço de Federação. Para obter mais informações, consulte [lista de verificação: configurar o AD FS declarações enviar para um AD FS 1. x serviço de Federação](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Qualquer serviço de Federação do AD FS no Windows Server 2012 pode enviar um AD FS 1. *x*\-compatible reivindicação que pode ser consumida por um ou mais servidores Web executando o AD FS 1. *x* claims\ reconhecimento agente de Web. Para obter mais informações, consulte [lista de verificação: configurar o AD FS declarações enviar para um AD FS 1. x agente da Web compatível com declarações](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
> [!NOTE]  
> AD FS não oferece suporte ou interoperar com o AD FS 1. *x* agente de Web baseado em token do Windows NT.  
  
Um AD FS 1. *x*\-compatible declaração é uma reivindicação que pode ser enviada por um serviço de Federação do AD FS no Windows Server 2012 e entendida por um AD FS 1. *x* serviço de Federação. Para que um AD FS 1. *x* serviço de Federação pode consumir as declarações que envia um serviço de Federação do AD FS, um tipo de declaração de nome identificador \(ID\) deve ser enviado.  
  
## <a name="understanding-the-name-id-claim-type"></a>Noções básicas sobre o tipo de declaração de nome ID  
O tipo de declaração de nome ID é o equivalente a declaração de identidade digite essa AD FS 1. *x* uses. Ele deve ser usado sempre que desejar interoperar com o AD FS 1. *x*. A declaração de nome ID digite permite ou um AD FS 1. *x* serviço de Federação ou o AD FS 1. *x* claims\ reconhecimento agente de Web para consumir declarações que envia o AD FS no Windows Server 2012, desde que essas declarações são enviadas em um dos formatos de nome ID na tabela a seguir.  
  
|Formato do nome do ID|URI correspondente|  
|------------------|---------------------|  
|AD FS 1. *x* endereço de Email|http://schemas.xmlsoap.org/claims/EmailAddress|  
|AD FS 1. *x* UPN de Email|http://schemas.xmlsoap.org/claims/UPN|  
|Nome comum|http://schemas.xmlsoap.org/claims/CommonName|  
|Grupo|http://schemas.xmlsoap.org/claims/Group|  
  
Somente uma declaração de nome ID no formato apropriado deve ser enviada. Quando esse critério estiver satisfeito, muitos outros requerimentos poderão ser enviados, supondo que eles estão em conformidade com as restrições descritas na tabela.  
  
> [!NOTE]  
> Um AD FS 1. *x* serviço de Federação pode interpretar apenas entrada tipos de declaração que começam com a \(URI\) Uniform Resource Identifier de http://schemas.xmlsoap.org/claims/.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
