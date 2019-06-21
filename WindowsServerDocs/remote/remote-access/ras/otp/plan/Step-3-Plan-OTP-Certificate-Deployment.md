---
title: Etapa 3 Planejar implantação de certificado OTP
description: Este tópico faz parte do guia de implantação de acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4102fc4f7eacf0b407446717caa83e4e5f70ae57
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282364"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>Etapa 3 Planejar implantação de certificado OTP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois de planejar o servidor RADIUS, você deve planejar os requisitos de autoridade (CA) de certificação, incluindo a autoridade de certificação que emite certificados de senha única (OTP), o modelo de certificado OTP e o certificado de autoridade de registro usado pelo controle remoto Servidor de acesso para assinar todas as solicitações de certificado OTP para cliente DirectAccess. Esses certificados são usados da seguinte maneira:  
  
1.  O cliente DirectAccess solicita um certificado OTP, e o servidor de acesso remoto recebe a solicitação.  
  
2.  O servidor de acesso remoto verifica as credenciais OTP e se eles forem válidos, o servidor atua como uma autoridade de registro e assina a solicitação de registro de certificado OTP usando um certificado de assinatura e de curta duração.  
  
3.  O servidor de acesso remoto envia a solicitação de registro de certificado para o cliente do DirectAccess  
  
4.  O cliente, em seguida, registra o certificado OTP da autoridade de certificação usando as solicitações de registro de certificado assinadas pelo servidor.  
  
5.  A autoridade de certificação verifica as credenciais e a solicitação.  
  
|Tarefa|Descrição|  
|----|--------|  
|[3.1 planejar a autoridade de certificação de OTP](#bkmk_3_1_CA)|Planeje a autoridade de certificação (CA) para usar para emitir certificados para os clientes DirectAccess para autenticação OTP.|  
|[3.2 planejar o modelo de certificado OTP](#bkmk_3_2_OTP_Cert)|Planeje o modelo de certificado OTP.|
|[3.3 planejar o certificado de autoridade de registro](#bkmk_33RACert)|Planeje o certificado de autoridade de registro para assinar todas as solicitações de certificado de autenticação de OTP.|

## <a name="bkmk_3_1_CA"></a>3.1 planejar a autoridade de certificação de OTP  
Para implantar o DirectAccess usando a autenticação de senha única (OTP), você precisa de uma CA interna emitir os certificados de autenticação de OTP para computadores cliente do DirectAccess. Para essa finalidade, você pode usar a mesma AC interna que você pode usar para emitir os certificados que são usados para autenticação de computador IPsec regular.  
  
## <a name="bkmk_3_2_OTP_Cert"></a>3.2 planejar o modelo de certificado OTP  
Cada cliente do DirectAccess requer um certificado de autenticação de OTP para obter acesso à rede interna. Você deve configurar um modelo em sua AC interna para o certificado OTP. Observe o seguinte ao configurar o modelo de certificado OTP:  
  
-   Todos os usuários que precisam para realizar a autenticação de OTP devem ter lido e permissões de registro para este modelo.  
  
-   O nome da entidade deve ser criado a partir de informações do Active Directory, para garantir que o nome da entidade corresponda o nome de usuário OTP e não o nome do servidor de acesso remoto que executa a solicitação de certificado. O nome da entidade deve estar no formato de nome totalmente distinto, e o nome alternativo da entidade deve estar no formato UPN. Isso garante que o certificado OTP registrado é válido para a autenticação Smartcard Kerberos.  
  
-   A finalidade do certificado deve ser um Logon de cartão inteligente  
  
-   Emissão deve exigir uma assinatura de autorizados. A assinatura deve ser configurada com a política de aplicativos de OTP DirectAccess predefinidos definido no modelo de certificado de assinatura de autoridade de registro.  
  
-   O período de validade deve ser definido para uma hora.  
  
    > [!NOTE]  
    > Em situações em que o servidor de autoridade de certificação é um computador com Windows Server 2003 e, em seguida, o modelo deve ser configurado em um computador diferente. Isso é devido ao fato de que definir a **período de validade** nas horas não é possível quando executando versões do Windows anteriores ao Vista/2008. Se o computador que você pode usar para configurar o modelo não tem a função do serviço de certificação instalada, ou é um computador cliente, em seguida, você talvez precise instalar o snap-in de modelos de certificado. Para obter mais informações sobre esse assunto, clique em [aqui](https://technet.microsoft.com/library/cc732445.aspx).  
  
-   O período de renovação deve ser definido como 0.  
  
-   (Opcional) Certificados e solicitações não devem ser armazenadas no banco de dados da autoridade de certificação.  
  
-   O parâmetro de uso avançado de chave do certificado deve ser definido corretamente, da seguinte maneira:  
  
    -   Para o modelo de certificado de assinatura do registro de DirectAccess, use a chave 1.3.6.1.4.1.311.81.1.1.  
  
    -   Para o modelo de certificado de autenticação de OTP, use a chave de chave 1.3.6.1.4.1.311.20.2.2.  
  
## <a name="bkmk_33RACert"></a>3.3 planejar o certificado de autoridade de registro  
Quando os clientes DirectAccess solicitam um certificado OTP, o servidor de acesso remoto recebe a solicitação do cliente. O servidor de acesso remoto faz todas as solicitações de certificado OTP de clientes usando o certificado de autoridade de registro. A autoridade de certificação emite certificados apenas se a solicitação é assinada pelo certificado de autoridade de registro no servidor de acesso remoto. O certificado deve ser emitido por uma CA interna, o certificado não pode ser autoassinado. Ele não precisa ser emitido pela autoridade de certificação que emitiu os certificados OTP, mas a autoridade de certificação que emite os certificados OTP deve confiar na AC que emite o certificado de assinatura de autoridade de registro.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 4: Planejar OTP para o servidor de acesso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


