---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Analisar a função do servidor de federação no parceiro de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841837"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Analisar a função do servidor de federação no parceiro de recurso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O servidor de federação na organização do parceiro de recurso intercepta tokens de segurança de entrada que são enviados por um servidor de federação de conta, valida e assina-os e, em seguida, emite seus próprios tokens de segurança que são destinados para a Web\-com base em aplicativo.  
  
> [!NOTE]  
> Quando os usuários federados usarem seus navegadores da Web para acessar a Web\-aplicativos baseados no, o servidor de federação na organização do parceiro de recurso cria um novo cookie de autenticação e grava-o no navegador. Esse cookie permite que o único\-sinal\-nos \(SSO\) recursos para que os usuários não precisam fazer logon novamente no servidor de federação no parceiro de conta quando os usuários tentarem acessar diferente Web\- aplicativos com base no parceiro de recurso.  
  
No design SSO da Web, pelo menos um servidor de federação deve ser instalado na rede de perímetro. No design SSO da Web federado, deve haver pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de conta e pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de recurso.  
  
> [!NOTE]  
> Antes de configurar um computador do servidor de federação na organização do parceiro de recurso, você primeiro deve ingressar o computador a qualquer domínio do Active Directory na organização do parceiro de recurso. Para obter mais informações, consulte [lista de verificação: Configurando um servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

