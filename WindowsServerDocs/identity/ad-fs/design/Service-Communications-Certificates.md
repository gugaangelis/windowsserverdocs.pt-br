---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificados de comunicações de serviço
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 91ef716f518676fe9cf42b2136afcbcd42c50946
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858519"
---
# <a name="service-communications-certificates"></a>Certificados de comunicações de serviço

Um servidor de Federação requer o uso de certificados de comunicação de serviço para cenários nos quais a segurança de mensagem do WCF é usada.  
  
## <a name="service-communication-certificate-requirements"></a>Requisitos de certificado de comunicação de serviço  
Os certificados de comunicação do serviço devem atender aos seguintes requisitos para trabalhar com AD FS:  
  
-   O certificado de comunicação do serviço deve incluir o uso avançado de chave de autenticação do servidor \(extensão de\) EKU.  
  
-   As listas de certificados revogados \(CRLs\) devem estar acessíveis para todos os certificados na cadeia do certificado de comunicação do serviço para o certificado de autoridade de certificação raiz. A CA raiz também deve ser confiável por qualquer proxy de servidor de Federação e servidores Web que confiam nesse servidor de Federação.  
  
-   O nome da entidade que é usado no certificado de comunicação do serviço deve corresponder ao nome do Serviço de Federação nas propriedades do Serviço de Federação.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considerações de implantação para certificados de comunicação de serviço  
Configure certificados de comunicação de serviço para que todos os servidores de Federação usem o mesmo certificado. Se você estiver implantando o\-de logon\-único da Web federado no design \(\) SSO, recomendamos que o certificado de comunicação do serviço seja emitido por uma autoridade de certificação pública. Você pode solicitar e instalar esses certificados por meio do snap\-do Gerenciador do IIS no.  
  
Você pode usar os certificados de comunicação de serviço auto\-assinados com êxito em servidores de Federação em um ambiente de laboratório de teste. No entanto, para um ambiente de produção, recomendamos que você obtenha certificados de comunicação de serviço de uma CA pública. Estas são as razões pelas quais você não deve usar os certificados de comunicação de serviço auto\-assinados para uma implantação dinâmica:  
  
-   Um certificado SSL auto\-assinado deve ser adicionado ao armazenamento de raiz confiável em cada um dos servidores de Federação na organização do parceiro de recurso. Embora isso sozinho não permita que um invasor comprometa um servidor de Federação de recursos, confiar em auto\-certificados assinados aumenta a superfície de ataque de um computador e pode levar a vulnerabilidades de segurança se o signatário do certificado não for confiável.  
  
-   Ele cria uma experiência de usuário inadequada. Os clientes receberão prompts de alerta de segurança quando tentarem acessar recursos federados que exibem a seguinte mensagem: "o certificado de segurança foi emitido por uma empresa que você não escolheu confiar". Esse é o comportamento esperado, pois o certificado auto\-assinado não é confiável.  
  
    > [!NOTE]  
    > Se necessário, você pode contornar essa condição usando Política de Grupo para enviar manualmente o certificado auto\-assinado para o repositório de raiz confiável em cada computador cliente que tentará acessar um site AD FS.  
  
-   As CAs fornecem recursos adicionais baseados em\-de certificado, como o arquivo de chave privada, a renovação e a revogação, que não são fornecidos por auto\-certificados assinados.  
  

