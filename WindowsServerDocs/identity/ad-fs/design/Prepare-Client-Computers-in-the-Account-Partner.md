---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparar os computadores cliente no parceiro de conta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5c20b3d58c4e70fdb72a4aa54f7a05bd7bb5c347
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparar os computadores cliente no parceiro de conta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A maneira mais fácil para um administrador em uma organização de parceiro de conta para preparar os computadores cliente para acesso aos serviços de Federação do Active Directory \(AD FS\) federados aplicativos é usar a política de grupo. Política de grupo oferece uma maneira conveniente para você por push de certificados específicos e as configurações necessárias para federação para todos os computadores cliente que será usado para acessar aplicativos federados.  
  
Para que os computadores cliente perfeitamente podem acessar aplicativos federados sem prompts do certificado ou avisos relacionados ao site confiáveis, recomendamos que você prepare primeiro cada computador cliente antes de implantar o AD FS amplamente em sua organização. Considere o uso de política de grupo para automaticamente:  
  
-   Configure o Internet Explorer em cada computador cliente para confiar no servidor de federação de conta.  
  
    Para obter mais informações, consulte [configurar computadores cliente para confiar no servidor de Federação conta](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Instalar o servidor de federação de conta apropriada, servidor de Federação do recurso e certificados de Secure Sockets Layer \(SSL\) do servidor Web \ (ou equivalente certificados que são encadeados para um root\ confiável) em cada computador cliente.  
  
    Para obter mais informações, consulte [distribuir certificados para computadores cliente usando a diretiva de grupo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
