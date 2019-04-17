---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Registrar um certificado SSL do AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 12f544ad0d037c4ae7a9789238186b7ded311bdf
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Registrar um certificado SSL do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Active Directory serviços de Federação \(AD FS\) requer um certificado de autenticação de servidor SSL \(SSL\) em cada servidor de federação no seu farm de servidores de Federação. O mesmo certificado pode ser usado em cada servidor de Federação em um farm. Você deve ter o certificado e sua chave privada disponível. Por exemplo, se você tiver o certificado e sua chave privada em um arquivo. pfx, você pode importar o arquivo diretamente para o Assistente de configuração dos serviços de Federação Active Directory. Esse certificado SSL deve conter o seguinte:  
  
1.  O nome do assunto e o nome do requerente alternativo devem conter seu nome de serviço de federação, como fs.contoso.com.  
  
2.  O nome do requerente alternativo deve conter o valor **enterpriseregistration** que é seguido pelo sufixo do nome UPN \(UPN\) da sua organização, por exemplo, **enterpriseregistration.corp.contoso.com**.  
  
    > [!WARNING]  
    > Se você pretende habilitar o serviço de registro de dispositivo \(DRS\) para Workplace Join, especifique o nome do requerente alternativo.  
  
> [!IMPORTANT]  
> Se sua organização usa vários sufixos UPN, e você pretende habilitar o DRS, o certificado SSL deve conter uma entrada de nome alternativas de assunto para cada sufixo.  
  
## <a name="see-also"></a>Consulte também
[AD FS implantação](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implantando um Farm de servidores de Federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

