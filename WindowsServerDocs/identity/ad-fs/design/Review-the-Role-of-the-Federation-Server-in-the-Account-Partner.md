---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Analise a função do proxy do servidor de federação no parceiro de conta
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2d51f0091746d2d5e816108e3b92d0cb1a8267cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858529"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Analise a função do proxy do servidor de federação no parceiro de conta

Um servidor de Federação no Serviços de Federação do Active Directory (AD FS) \(AD FS funções\) como um emissor de token de segurança. Um servidor de Federação gera declarações baseadas em valores de conta que residem em um repositório de atributos local e os empacota em tokens de segurança para que os usuários possam acessar diretamente os aplicativos baseados no Web\-browser\-\(usando o\-de logon único em \(SSO\)\) hospedados em uma organização de parceiro de recurso.  
  
> [!NOTE]  
> Quando os usuários acessam aplicativos federados usando um navegador da Web, um servidor de Federação emite automaticamente cookies para os usuários para manter o status de logon desse aplicativo Web\-navegador\-. Esses cookies incluem declarações para os usuários. Os cookies habilitam recursos de SSO para que os usuários não precisem inserir as credenciais toda vez que visitarem aplicativos Web\-navegador diferentes\-baseados no parceiro de recurso.  
  
No design de SSO da Web, as organizações com uma rede de perímetro que desejam que os usuários da Internet tenham acesso aos aplicativos devem instalar um proxy de servidor de Federação na rede de perímetro. No design de SSO da Web federado, deve haver pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de conta e pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de recurso.  
  
> [!NOTE]  
> Antes de configurar um computador do servidor de Federação na organização do parceiro de conta, você deve primeiro ingressar o computador em qualquer domínio na floresta Active Directory em que o servidor de Federação será usado para autenticar usuários dessa floresta. Para obter mais informações, consulte [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
