---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparar computadores cliente no parceiro de conta
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 014524c78312c6fcd478b40ec47e212a194b0531
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858589"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparar computadores cliente no parceiro de conta

A maneira mais fácil de um administrador em uma organização de parceiro de conta preparar os computadores cliente para acesso a Serviços de Federação do Active Directory (AD FS) \(AD FS\) aplicativos federados é usar Política de Grupo. A Política de Grupo fornece uma maneira conveniente de você enviar por push configurações e certificados específicos necessários para a federação para todos os computadores cliente que serão usados para acessar os aplicativos federados.  
  
Para que os computadores cliente possam acessar diretamente aplicativos federados sem prompts de certificado ou prompts relacionados ao site confiável, recomendamos que você primeiro Prepare cada computador cliente antes de implantar AD FS amplamente em sua organização. Considere usar a Política de Grupo para automaticamente:  
  
-   Configure o Internet Explorer em cada computador cliente para confiar no servidor de Federação da conta.  
  
    Para obter mais informações, consulte [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Instale o servidor de Federação de conta apropriado, o servidor de Federação de recursos e o servidor Web protocolo SSL \(SSL\) certificados \(ou certificados equivalentes que se encadeadom a um\) de raiz confiável em cada computador cliente.  
  
    Para obter mais informações, consulte [distribuir certificados para computadores cliente usando política de grupo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
