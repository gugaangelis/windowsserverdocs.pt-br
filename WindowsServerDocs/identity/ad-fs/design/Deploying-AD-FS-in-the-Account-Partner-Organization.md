---
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: "Implantando o AD FS na organização do parceiro de conta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5b4ba00aa9fed1022d9c0137d05ac6240b44b276
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Implantando o AD FS na organização do parceiro de conta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Um parceiro de conta em serviços de Federação do Active Directory \(AD FS\) representa a organização na relação de confiança de federação que armazena contas de usuário fisicamente em um repositório de atributo com suporte. Para obter mais informações sobre quais atributo lojas são compatíveis, consulte [função The do atributo armazena](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
O servidor de federação na organização do parceiro de conta autentica usuários locais e cria tokens de segurança que são usados pelo parceiro de recurso na tomada de decisões de autorização. Partes confiantes, como sites e serviços da Web, em seguida, são capazes de se registrar com o servidor de Federação e consumir facilmente emitido tokens de autenticação e controle de acesso.  
  
Em cenários em que você precisa fornecer aos usuários acesso a vários aplicativos federados ou serviços — quando cada aplicativo ou serviço está hospedado por uma organização diferente — você pode configurar o servidor de Federação do parceiro de conta para que você pode implantar várias partes confiantes.  
  
Para obter mais informações sobre como instalar e configurar uma organização de parceiro de conta, consulte [lista de verificação: Configurando a organização do parceiro de conta](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Examine a função do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Examine a função do Proxy do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Preparar os computadores cliente no parceiro de conta](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
