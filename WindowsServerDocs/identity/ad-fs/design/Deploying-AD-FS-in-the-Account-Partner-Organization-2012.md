---
ms.assetid: 9aaca9c5-ce44-495c-aad6-61aede87a83f
title: Implantando o AD FS na organização do parceiro de conta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 62d7549f124b96cd7addf7e54cc5d0c1d9897098
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408120"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Implantando o AD FS na organização do parceiro de conta

Um parceiro de conta no Serviços de Federação do Active Directory (AD FS) \(AD FS\) representa a organização na relação de confiança da Federação que armazena fisicamente as contas de usuário em um repositório de atributos com suporte. Para obter mais informações sobre quais repositórios de atributos têm suporte, consulte [a função de repositórios de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
O servidor de Federação na organização do parceiro de conta autentica usuários locais e cria tokens de segurança que são usados pelo parceiro de recurso para tomar decisões de autorização. Partes confiáveis, como sites e Web Services, são capazes de se registrar facilmente no servidor de Federação e consumir tokens emitidos para autenticação e controle de acesso.  
  
Em cenários em que você precisa fornecer aos usuários acesso a vários aplicativos ou serviços federados — quando cada aplicativo ou serviço é hospedado por uma organização diferente — você pode configurar o servidor de Federação de parceiro de conta para que possa implantar várias partes confiáveis.  
  
Para obter mais informações sobre como instalar e configurar a organização do parceiro de conta, consulte [Checklist: Configuring the Account Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Analisar a função do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Analisar a função do proxy do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Preparar computadores cliente no parceiro de conta](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
