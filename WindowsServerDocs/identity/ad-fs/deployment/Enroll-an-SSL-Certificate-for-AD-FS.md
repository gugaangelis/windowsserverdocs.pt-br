---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Registrar um certificado SSL do AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6f7af40f23c3fa3bd0a31ecb74b11013133a4b32
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855429"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Registrar um certificado SSL do AD FS

Serviços de Federação do Active Directory (AD FS) \(AD FS\) requer um certificado para a autenticação do Secure Socket Layer \(SSL\) Server em cada servidor de Federação em seu farm de servidores de Federação. O mesmo certificado pode ser usado em cada servidor de Federação em um farm. Você deve ter o certificado e sua chave privada disponíveis. Por exemplo, se você tem o certificado e a chave privada em um arquivo .pfx, pode importar o arquivo diretamente para o Assistente de Configuração de Serviços de Federação do Active Directory. Esse certificado SSL deve conter o seguinte:  
  
1.  O nome da entidade e o nome alternativo da entidade devem conter o nome do serviço de Federação, como fs.contoso.com.  
  
2.  O nome alternativo da entidade deve conter o valor **enterpriseregistration** seguido pelo nome principal do usuário \(sufixo de\) UPN da sua organização, por exemplo, **enterpriseregistration.Corp.contoso.com**.  
  
    > [!WARNING]  
    > Especifique o nome alternativo da entidade se você planeja habilitar o serviço de registro de dispositivo \(DRS\) para Workplace Join.  
  
> [!IMPORTANT]  
> Se sua organização usa vários sufixos UPN e você planeja habilitar o DRS, o certificado SSL deve conter uma entrada de nome alternativo da entidade para cada sufixo.  
  
## <a name="see-also"></a>Consulte também
[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

