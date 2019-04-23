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
ms.openlocfilehash: 0c5bdcb0a80b15a1905109229ddd20ee642a8dd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868517"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparar computadores cliente no parceiro de conta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A maneira mais fácil para um administrador em uma conta de organização para preparar os computadores cliente para o acesso aos serviços de Federação do Active Directory do parceiro \(do AD FS\) aplicativos federados é usar a diretiva de grupo. A Política de Grupo fornece uma maneira conveniente de você enviar por push configurações e certificados específicos necessários para a federação para todos os computadores cliente que serão usados para acessar os aplicativos federados.  
  
Para que os computadores cliente podem acessar diretamente os aplicativos federados sem prompts de certificado ou prompts de relacionados ao site confiáveis, é recomendável primeiro preparar cada computador cliente antes de implantar o AD FS em larga escala em sua organização. Considere usar a Política de Grupo para automaticamente:  
  
-   Configure o Internet Explorer em cada computador cliente para confiar no servidor de federação de conta.  
  
    Para obter mais informações, consulte [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Instalar o servidor de federação de conta apropriado, o servidor de federação de recurso e o servidor Web Secure Sockets Layer \(SSL\) certificados \(ou o equivalente em certificados vinculados a uma raiz confiável\) em cada computador cliente.  
  
    Para obter mais informações, consulte [distribuir certificados para computadores cliente usando a diretiva de grupo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
