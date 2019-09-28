---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Analise a função do proxy do servidor de federação no parceiro de conta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ead8868f38faa570a0e524630e23d99e276a7c79
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407919"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Analise a função do proxy do servidor de federação no parceiro de conta

Um servidor de Federação nas funções Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 como um emissor de token de segurança. Um servidor de Federação gera declarações com base em valores de conta que residem em um repositório de atributos local e os empacota em tokens de segurança para que os usuários possam acessar diretamente os aplicativos Web @ no__t-0browser @ no__t-1based \(Using logon único @ no__t-3on \(SSO @ no__t-5 @ no__t-6 hospedados em uma organização de parceiro de recurso.  
  
> [!NOTE]  
> Quando os usuários acessam aplicativos federados usando um navegador da Web, um servidor de Federação emite automaticamente cookies para os usuários para manter seu status de logon para esse aplicativo Web @ no__t-0browser @ no__t-1based. Esses cookies incluem declarações para os usuários. Os cookies habilitam recursos de SSO para que os usuários não precisem inserir credenciais toda vez que visitarem aplicativos Web @ no__t-0browser @ no__t-1based diferentes no parceiro de recurso.  
  
No design de SSO da Web, as organizações com uma rede de perímetro que desejam que os usuários da Internet tenham acesso aos aplicativos devem instalar um proxy de servidor de Federação na rede de perímetro. No design de SSO da Web federado, deve haver pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de conta e pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de recurso.  
  
> [!NOTE]  
> Antes de configurar um computador do servidor de Federação na organização do parceiro de conta, você deve primeiro ingressar o computador em qualquer domínio na floresta Active Directory em que o servidor de Federação será usado para autenticar usuários dessa floresta. Para obter mais informações, consulte [Checklist: Configurando um servidor de Federação @ no__t-0.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
