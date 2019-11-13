---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Analisar a função do servidor de federação no parceiro de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fd9f20eb7559f5862ee50bdd8364fa1604d3c1b6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358972"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Analisar a função do servidor de federação no parceiro de recurso

O servidor de Federação na organização do parceiro de recurso intercepta tokens de segurança de entrada que são enviados por um servidor de Federação de conta, valida-os e os assina e, em seguida, emite seus próprios tokens de segurança destinados ao aplicativo baseado na Web\-.  
  
> [!NOTE]  
> Quando usuários federados usam seus navegadores da Web para acessar aplicativos baseados na Web\-, o servidor de Federação na organização do parceiro de recurso cria um novo cookie de autenticação e grava-o no navegador. Esse cookie habilita o logon único de\-\-em recursos de\) SSO \(para que os usuários não precisem fazer logon novamente no servidor de Federação no parceiro de conta quando os usuários tentarem acessar diferentes aplicativos baseados na Web\-no parceiro de recurso.  
  
No design de SSO da Web, pelo menos um servidor de Federação deve ser instalado na rede de perímetro. No design de SSO da Web federado, deve haver pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de conta e pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de recurso.  
  
> [!NOTE]  
> Antes de configurar um computador do servidor de Federação na organização do parceiro de recurso, você deve primeiro ingressar o computador em qualquer Active Directory domínio na organização do parceiro de recurso. Para obter mais informações, consulte [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

