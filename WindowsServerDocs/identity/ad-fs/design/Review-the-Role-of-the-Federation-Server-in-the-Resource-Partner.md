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
ms.openlocfilehash: b2ed7a09bbc50c83d3bf6f8f2688152ed5202abc
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190823"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Analisar a função do servidor de federação no parceiro de recurso

O servidor de federação na organização do parceiro de recurso intercepta tokens de segurança de entrada que são enviados por um servidor de federação de conta, valida e assina-os e, em seguida, emite seus próprios tokens de segurança que são destinados para a Web\-com base em aplicativo.  
  
> [!NOTE]  
> Quando os usuários federados usarem seus navegadores da Web para acessar a Web\-aplicativos baseados no, o servidor de federação na organização do parceiro de recurso cria um novo cookie de autenticação e grava-o no navegador. Esse cookie permite que o único\-sinal\-nos \(SSO\) recursos para que os usuários não precisam fazer logon novamente no servidor de federação no parceiro de conta quando os usuários tentarem acessar diferente Web\- aplicativos com base no parceiro de recurso.  
  
No design SSO da Web, pelo menos um servidor de federação deve ser instalado na rede de perímetro. No design SSO da Web federado, deve haver pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de conta e pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de recurso.  
  
> [!NOTE]  
> Antes de configurar um computador do servidor de federação na organização do parceiro de recurso, você primeiro deve ingressar o computador a qualquer domínio do Active Directory na organização do parceiro de recurso. Para obter mais informações, consulte [lista de verificação: Configurando um servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

