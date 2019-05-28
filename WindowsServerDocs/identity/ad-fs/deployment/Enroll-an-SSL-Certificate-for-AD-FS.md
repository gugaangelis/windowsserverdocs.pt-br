---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Registrar um certificado SSL do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e80927f2670614d2949f4e67cc158319f05c5fa0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192148"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Registrar um certificado SSL do AD FS

Serviços de Federação do Active Directory \(do AD FS\) requer um certificado para Secure Socket Layer \(SSL\) autenticação de servidor em cada servidor de federação no seu farm de servidores de Federação. O mesmo certificado pode ser usado em cada servidor de Federação em um farm. Você deve ter o certificado e a respectiva chave privada disponíveis. Por exemplo, se você tem o certificado e a chave privada em um arquivo .pfx, pode importar o arquivo diretamente para o Assistente de Configuração de Serviços de Federação do Active Directory. O certificado SSL deve conter o seguinte:  
  
1.  O nome da entidade e o nome alternativo da entidade devem conter o nome do serviço de federação, por exemplo, fs.contoso.com.  
  
2.  Nome alternativo da entidade deve conter o valor **enterpriseregistration** que é seguido pelo nome UPN \(UPN\) sufixo da sua organização, por exemplo,  **enterpriseregistration.corp.contoso.com**.  
  
    > [!WARNING]  
    > Especifique o nome alternativo da entidade se você planeja habilitar o serviço de registro de dispositivo \(DRS\) para ingresso no local.  
  
> [!IMPORTANT]  
> Se sua organização usa vários sufixos UPN, e você planeja habilitar o DRS, o certificado SSL deve conter uma entrada de nome alternativo da entidade para cada sufixo.  
  
## <a name="see-also"></a>Consulte também
[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

