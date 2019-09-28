---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificados de comunicações de serviço
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 624d2e26bc0277129e44eee3fdce7c7396b735a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407909"
---
# <a name="service-communications-certificates"></a>Certificados de comunicações de serviço

Um servidor de Federação requer o uso de certificados de comunicação de serviço para cenários nos quais a segurança de mensagem do WCF é usada.  
  
## <a name="service-communication-certificate-requirements"></a>Requisitos de certificado de comunicação de serviço  
Os certificados de comunicação do serviço devem atender aos seguintes requisitos para trabalhar com AD FS:  
  
-   O certificado de comunicação do serviço deve incluir a extensão uso avançado de chave de autenticação do servidor \(EKU @ no__t-1.  
  
-   As listas de revogação de certificado \(CRLs @ no__t-1 devem estar acessíveis para todos os certificados na cadeia do certificado de comunicação de serviço para o certificado de autoridade de certificação raiz. A CA raiz também deve ser confiável por qualquer proxy de servidor de Federação e servidores Web que confiam nesse servidor de Federação.  
  
-   O nome da entidade que é usado no certificado de comunicação do serviço deve corresponder ao nome do Serviço de Federação nas propriedades do Serviço de Federação.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considerações de implantação para certificados de comunicação de serviço  
Configure certificados de comunicação de serviço para que todos os servidores de Federação usem o mesmo certificado. Se você estiver implantando a Web federada single @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 design, recomendamos que o certificado de comunicação do serviço seja emitido por uma AC pública. Você pode solicitar e instalar esses certificados usando o snap @ no__t-0in do Gerenciador do IIS.  
  
Você pode usar Self @ no__t-0signed, certificados de comunicação de serviço com êxito em servidores de Federação em um ambiente de laboratório de teste. No entanto, para um ambiente de produção, recomendamos que você obtenha certificados de comunicação de serviço de uma CA pública. A seguir, os motivos pelos quais você não deve usar o Self @ no__t-0signed, certificados de comunicação de serviço para uma implantação ao vivo:  
  
-   R Self @ no__t-0signed, o certificado SSL deve ser adicionado ao armazenamento de raiz confiável em cada um dos servidores de Federação na organização do parceiro de recurso. Embora isso sozinho não permita que um invasor comprometa um servidor de Federação de recursos, confiar nos certificados autono__t-0signed aumenta a superfície de ataque de um computador e pode levar a vulnerabilidades de segurança se o signatário do certificado não estiver confiável.  
  
-   Ele cria uma experiência de usuário inadequada. Os clientes receberão prompts de alerta de segurança quando tentarem acessar recursos federados que exibem a seguinte mensagem: "O certificado de segurança foi emitido por uma empresa que você não escolheu confiar". Esse é o comportamento esperado, pois o certificado @ no__t-0signed não é confiável.  
  
    > [!NOTE]  
    > Se necessário, você pode contornar essa condição usando Política de Grupo para enviar manualmente o certificado autono__t-0signed para o repositório de raiz confiável em cada computador cliente que tentará acessar um site de AD FS.  
  
-   O CAs fornece recursos adicionais de certificado @ no__t-0based, como o arquivo de chave privada, a renovação e a revogação, que não são fornecidos pelos certificados Self @ no__t-1signed.  
  

