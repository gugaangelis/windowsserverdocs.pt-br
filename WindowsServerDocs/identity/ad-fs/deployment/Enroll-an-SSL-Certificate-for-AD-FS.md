---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Registrar um certificado SSL do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: efa7c7aee848a5bbb68d3ce7140e135d37c2161d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408367"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Registrar um certificado SSL do AD FS

Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 requer um certificado para a autenticação de servidor do Secure Socket Layer \(SSL @ no__t-3 em cada servidor de Federação em seu farm de servidores de Federação. O mesmo certificado pode ser usado em cada servidor de Federação em um farm. Você deve ter o certificado e a respectiva chave privada disponíveis. Por exemplo, se você tem o certificado e a chave privada em um arquivo .pfx, pode importar o arquivo diretamente para o Assistente de Configuração de Serviços de Federação do Active Directory. O certificado SSL deve conter o seguinte:  
  
1.  O nome da entidade e o nome alternativo da entidade devem conter o nome do serviço de Federação, como fs.contoso.com.  
  
2.  O nome alternativo da entidade deve conter o valor **enterpriseregistration** que é seguido pelo nome principal do usuário \(UPN @ no__t-2 sufixo de sua organização, por exemplo, **enterpriseregistration.Corp.contoso.com**.  
  
    > [!WARNING]  
    > Especifique o nome alternativo da entidade se você planeja habilitar o serviço de registro de dispositivo \(DRS @ no__t-1 para Workplace Join.  
  
> [!IMPORTANT]  
> Se sua organização usa vários sufixos UPN e você planeja habilitar o DRS, o certificado SSL deve conter uma entrada de nome alternativo da entidade para cada sufixo.  
  
## <a name="see-also"></a>Consulte também
[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

