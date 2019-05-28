---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificados de comunicações de serviço
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b781d6fe99864b13d6e7f8ab65f3a14d205c2aa6
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190796"
---
# <a name="service-communications-certificates"></a>Certificados de comunicações de serviço

Um servidor de Federação requer o uso de certificados de comunicação de serviço para cenários em que a segurança de mensagem do WCF é usada.  
  
## <a name="service-communication-certificate-requirements"></a>Requisitos de certificado de comunicação de serviço  
Certificados de comunicação de serviço devem atender aos requisitos a seguir para trabalhar com o AD FS:  
  
-   O certificado de comunicação de serviço deve incluir o uso de chave de autenticação aprimorada do servidor \(EKU\) extensão.  
  
-   As listas de certificados revogados \(CRLs\) devem estar acessíveis para todos os certificados na cadeia do certificado de comunicação de serviço para o certificado de autoridade de certificação raiz. A CA raiz também deve ser confiável por qualquer proxies de servidor de Federação e servidores Web que confiam neste servidor de Federação.  
  
-   O nome da entidade que é usado no certificado de comunicação de serviço deve corresponder ao nome do serviço de Federação nas propriedades do serviço de Federação.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considerações de implantação de certificados de comunicação de serviço  
Configure certificados de comunicação de serviço para que todos os servidores de federação usam o mesmo certificado. Se você estiver implantando a Federated Web único\-sinal\-nos \(SSO\) design, é recomendável que seu certificado de comunicação do serviço sejam emitidos por uma autoridade de certificação pública. Você pode solicitar e instalar esses certificados por meio do Gerenciador do IIS snap\-no.  
  
Você pode usar o autoatendimento\-assinado, certificados de comunicação com êxito nos servidores de Federação em um ambiente de laboratório de teste de serviço. No entanto, para um ambiente de produção, recomendamos que você obtenha certificados de comunicação de serviço de uma CA pública. Estes são os motivos por que você não deve usar self\-assinado, certificados de comunicação para uma implantação em tempo real de serviço:  
  
-   Um self\-assinado, certificado SSL deve ser adicionado ao repositório raiz confiável em cada um dos servidores de federação na organização do parceiro de recurso. Enquanto isso sozinho não permitir que um invasor comprometer um servidor de federação de recurso, confiando self\-certificados autoassinados aumentar a superfície de ataque de um computador, e pode levar a vulnerabilidades de segurança se o certificado de signatário não é confiável.  
  
-   Ele cria uma experiência de usuário ruim. Os clientes receberão prompts de alerta de segurança quando eles tentam acessar recursos federados que exibem a seguinte mensagem: "O certificado de segurança foi emitido por uma empresa que você escolheu não confiar". Esse comportamento é esperado, pois o self\-certificado autoassinado não é confiável.  
  
    > [!NOTE]  
    > Se necessário, você pode contornar essa condição, usando a diretiva de grupo para empurrar manualmente o self\-assinou o certificado ao repositório raiz confiável em cada computador cliente que tenta acessar um site do AD FS.  
  
-   As CAs fornecem certificado adicional\-com base em recursos, como arquivamento da chave privado, renovação e revogação, que não são fornecidos pelo próprio\-certificados autoassinados.  
  

