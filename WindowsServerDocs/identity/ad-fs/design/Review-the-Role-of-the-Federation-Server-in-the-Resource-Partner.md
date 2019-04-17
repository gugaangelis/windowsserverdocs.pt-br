---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: "Examine a função do servidor de federação no parceiro de recurso"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Examine a função do servidor de federação no parceiro de recurso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O servidor de federação no recurso parceiro organização intercepta entrada tokens de segurança que são enviados por um servidor de federação de conta, valida assina-los e depois, emite seus próprio tokens de segurança que são destinados para o aplicativo com base em Web\.  
  
> [!NOTE]  
> Quando os usuários federados usarem seus navegadores da Web para acessar aplicativos baseados em Web\, o servidor de federação na organização do parceiro de recurso cria um novo cookie de autenticação e grava o navegador. Esse cookie permite single\ sign\-\(SSO\) recursos para que os usuários não precisam fazer logon novamente no servidor de federação no parceiro de conta quando os usuários tentam acessar diferentes aplicativos baseados em Web\ no parceiro de recurso.  
  
O design de SSO da Web, servidor de Federação pelo menos um deve ser instalado na rede de perímetro. O design de SSO da Web federado, deve haver pelo menos um servidor de Federação instalados na rede corporativa da organização do parceiro de conta e pelo menos um servidor de Federação instalados na rede corporativa da organização do parceiro de recurso.  
  
> [!NOTE]  
> Antes de você pode configurar um computador do servidor de federação na organização do parceiro de recurso, primeiro você deve ingressar o computador em qualquer domínio do Active Directory na organização do parceiro de recurso. Para obter mais informações, consulte [lista de verificação: configuração de um servidor federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

