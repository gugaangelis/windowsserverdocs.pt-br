---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: "Certificados de comunicações de serviço"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 32e60f2c2d9e4fced04061ace44882b792e1a3bc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="service-communications-certificates"></a>Certificados de comunicações de serviço

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um servidor de Federação requer o uso de certificados de comunicação de serviço para cenários nos quais a segurança de mensagem do WCF é usada.  
  
## <a name="service-communication-certificate-requirements"></a>Requisitos de certificado de comunicação do serviço  
Certificados de comunicação do serviço devem atender aos seguintes requisitos para trabalhar com o AD FS:  
  
-   O certificado de comunicação de serviço deve incluir a extensão \(EKU\) de uso da chave de autenticação aprimorada do servidor.  
  
-   O certificado revogação listas \(CRLs\) devem estar acessíveis para todos os certificados na cadeia do certificado de comunicação de serviço para o certificado da CA raiz. A CA raiz também deve ser confiável pelos qualquer proxies de servidor de Federação e servidores Web que confiar nesse servidor de Federação.  
  
-   O nome do requerente que é usado no certificado de comunicação de serviço deve corresponder ao nome do serviço de Federação nas propriedades do serviço de Federação.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considerações sobre a implantação de certificados de comunicação do serviço  
Configure certificados de comunicação de serviço para que todos os servidores de federação usam o mesmo certificado. Se você estiver implantando o design federados da Web Single\-Sign\-On \(SSO\), recomendamos que o certificado de comunicação do serviço ser emitido por uma autoridade de certificação pública. Você pode solicitar e instalar esses certificados por meio do Gerenciador do IIS snap\-in.  
  
Você pode usar self\ assinado, comunicação de serviço com êxito certificados nos servidores de Federação em um ambiente de laboratório de teste. No entanto, para um ambiente de produção, recomendamos que você obtenha os certificados de comunicação do serviço de uma autoridade de certificação pública. Estes são os motivos por que você deve não use self\ assinado, serviço comunicação certificados para uma implantação dinâmico:  
  
-   A self\ assinado, certificado SSL deve ser adicionado ao repositório raiz confiável em cada um dos servidores de federação na organização do parceiro de recurso. Enquanto isso sozinho não permitir que um invasor comprometa um servidor de Federação do recurso, confiar em certificados assinados self\ aumentar a superfície de ataque de um computador, e ele pode levar a vulnerabilidades de segurança se o signatário do certificado não é confiável.  
  
-   Ele cria uma experiência de usuário ruim. Os clientes receberão prompts de alerta de segurança quando eles tentam acessar recursos federados que exibem a seguinte mensagem: "o certificado de segurança foi emitido por uma empresa que decidiu não confiar em". Este é o comportamento esperado, porque o certificado assinado self\ não é confiável.  
  
    > [!NOTE]  
    > Se necessário, você pode contornar essa condição usando a política de grupo para enviar por push manualmente para baixo o certificado assinado self\ ao repositório raiz confiável em cada computador cliente que tenta acessar um site do AD FS.  
  
-   CAs fornecem com base em certificate\ recursos adicionais, como arquivamento de chave particular, renovação e revogação, que não são fornecidos por certificados assinados self\.  
  

