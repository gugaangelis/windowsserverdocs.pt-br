---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparar computadores cliente no parceiro de conta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e3a746ec003cf312ffe0b9804f84a55c98aa8089
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190995"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparar computadores cliente no parceiro de conta

A maneira mais fácil para um administrador em uma conta de organização para preparar os computadores cliente para o acesso aos serviços de Federação do Active Directory do parceiro \(do AD FS\) aplicativos federados é usar a diretiva de grupo. A Política de Grupo fornece uma maneira conveniente de você enviar por push configurações e certificados específicos necessários para a federação para todos os computadores cliente que serão usados para acessar os aplicativos federados.  
  
Para que os computadores cliente podem acessar diretamente os aplicativos federados sem prompts de certificado ou prompts de relacionados ao site confiáveis, é recomendável primeiro preparar cada computador cliente antes de implantar o AD FS em larga escala em sua organização. Considere usar a Política de Grupo para automaticamente:  
  
-   Configure o Internet Explorer em cada computador cliente para confiar no servidor de federação de conta.  
  
    Para obter mais informações, consulte [configurar computadores de cliente para confiar no servidor de federação de conta](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Instalar o servidor de federação de conta apropriado, o servidor de federação de recurso e o servidor Web Secure Sockets Layer \(SSL\) certificados \(ou o equivalente em certificados vinculados a uma raiz confiável\) em cada computador cliente.  
  
    Para obter mais informações, consulte [distribuir certificados para computadores cliente usando a diretiva de grupo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
