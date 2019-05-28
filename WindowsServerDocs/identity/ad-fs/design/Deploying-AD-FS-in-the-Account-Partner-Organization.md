---
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: Implantando o AD FS na organização do parceiro de conta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 93f61bc7fd147b2e0220178bcd163b6ca56279cf
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191572"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Implantando o AD FS na organização do parceiro de conta

Um parceiro de conta nos serviços de Federação do Active Directory \(do AD FS\) representa a organização na relação de confiança de federação que armazena fisicamente as contas de usuário em um repositório de atributos com suporte. Para obter mais informações sobre qual atributo armazenamentos com suporte, consulte [função The dos repositórios de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
O servidor de federação na organização do parceiro de conta autentica os usuários locais e cria tokens de segurança que são usados pelo parceiro de recurso na tomada de decisões de autorização. Terceiras partes confiáveis, como sites da Web e serviços da Web, em seguida, são capazes de facilmente se registram com o servidor de Federação e consomem tokens emitidos para autenticação e controle de acesso.  
  
Em cenários em que você precisa fornecer aos usuários acesso a vários aplicativos federados ou serviços — quando cada aplicativo ou serviço é hospedado por uma organização diferente — você pode configurar o servidor de Federação do parceiro de conta para que você possa implantar várias partes confiáveis.  
  
Para obter mais informações sobre como instalar e configurar uma organização do parceiro de conta, consulte [lista de verificação: Configurando a organização do parceiro de conta](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Analisar a função do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Analisar a função do proxy do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Preparar os computadores cliente no parceiro de conta](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
