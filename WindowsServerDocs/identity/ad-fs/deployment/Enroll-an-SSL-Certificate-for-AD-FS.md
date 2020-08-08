---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Registrar um certificado SSL do AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2d24e5593af2d12359d4e2acd5fc93df334730b8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962820"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Registrar um certificado SSL do AD FS

Serviços de Federação do Active Directory (AD FS) \( AD FS \) requer um certificado para a autenticação do servidor SSL da camada de soquete seguro \( \) em cada servidor de Federação em seu farm de servidores de Federação. O mesmo certificado pode ser usado em cada servidor de Federação em um farm. Você deve ter o certificado e a respectiva chave privada disponíveis. Por exemplo, se você tem o certificado e a chave privada em um arquivo .pfx, pode importar o arquivo diretamente para o Assistente de Configuração de Serviços de Federação do Active Directory. O certificado SSL deve conter o seguinte:

1.  O nome da entidade e o nome alternativo da entidade devem conter o nome do serviço de Federação, como fs.contoso.com.

2.  O nome alternativo da entidade deve conter o valor **enterpriseregistration** que é seguido pelo \( sufixo UPN do nome principal do usuário \) da sua organização, por exemplo, **enterpriseregistration.Corp.contoso.com**.

    > [!WARNING]
    > Especifique o nome alternativo da entidade se você planeja habilitar o DRS do serviço \( \) de registro de dispositivo para Workplace join.

> [!IMPORTANT]
> Se sua organização usa vários sufixos UPN e você planeja habilitar o DRS, o certificado SSL deve conter uma entrada de nome alternativo da entidade para cada sufixo.

## <a name="see-also"></a>Consulte Também
[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guia de Implantação do AD FS do Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)



