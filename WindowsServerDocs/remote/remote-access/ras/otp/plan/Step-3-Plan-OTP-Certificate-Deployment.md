---
title: Etapa 3 planejar a implantação do certificado OTP
description: Este tópico faz parte do guia implantar o acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8406286599e5b03173ce1b5d6c34c35245a9094
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366951"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>Etapa 3 planejar a implantação do certificado OTP

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Depois de planejar o servidor RADIUS, você deve planejar os requisitos de AC (autoridade de certificação), incluindo a AC que emitirá certificados de OTP (senha de uso único), o modelo de certificado de OTP e o certificado de autoridade de registro usado pelo remoto Servidor de acesso para assinar todas as solicitações de certificado de OTP do cliente do DirectAccess. Esses certificados são usados da seguinte maneira:  
  
1.  O cliente do DirectAccess solicita um certificado de OTP e o servidor de acesso remoto recebe a solicitação.  
  
2.  O servidor de acesso remoto verifica as credenciais de OTP e, se elas são válidas, o servidor atua como uma autoridade de registro e assina a solicitação de registro de certificado OTP usando um certificado de assinatura de curta duração.  
  
3.  O servidor de acesso remoto envia a solicitação de registro de certificado assinado de volta para o cliente do DirectAccess  
  
4.  O cliente registra o certificado de OTP da AC usando as solicitações de registro de certificado assinadas pelo servidor.  
  
5.  A autoridade de certificação verifica as credenciais e a solicitação.  
  
|Tarefa|Descrição|  
|----|--------|  
|[3,1 planejar a AC de OTP](#bkmk_3_1_CA)|Planeje a AC (autoridade de certificação) a ser usada para emitir certificados para clientes DirectAccess para autenticação OTP.|  
|[3,2 planejar o modelo de certificado de OTP](#bkmk_3_2_OTP_Cert)|Planeje o modelo de certificado de OTP.|
|[3,3 planejar o certificado de autoridade de registro](#bkmk_33RACert)|Planeje o certificado de autoridade de registro para assinar todas as solicitações de certificado de autenticação OTP.|

## <a name="bkmk_3_1_CA"></a>3,1 planejar a AC de OTP  
Para implantar o DirectAccess usando a OTP (autenticação de senha única), você precisa de uma CA interna para emitir os certificados de autenticação de OTP para os computadores cliente do DirectAccess. Para essa finalidade, você pode usar a mesma AC interna que você usa para emitir os certificados que são usados para autenticação regular do computador IPsec.  
  
## <a name="bkmk_3_2_OTP_Cert"></a>3,2 planejar o modelo de certificado de OTP  
Cada cliente do DirectAccess requer um certificado de autenticação de OTP para obter acesso à rede interna. Você deve configurar um modelo em sua autoridade de certificação interna para o certificado de OTP. Observe o seguinte ao configurar o modelo de certificado de OTP:  
  
-   Todos os usuários que precisam executar a autenticação OTP devem ter permissões de leitura e registro para este modelo.  
  
-   O nome da entidade deve ser criado a partir de Active Directory informações, para garantir que o nome da entidade corresponda ao nome de usuário da OTP e não ao nome do servidor de acesso remoto que executa a solicitação de certificado. O nome da entidade deve estar no formato de nome totalmente distinto e o nome alternativo da entidade deve estar no formato UPN. Isso garante que o certificado de OTP registrado seja válido para a autenticação Kerberos de cartão inteligente.  
  
-   A finalidade pretendida do certificado deve ser logon de cartão inteligente  
  
-   A emissão deve exigir uma assinatura autorizada. A assinatura deve ser configurada com a política de aplicativo de OTP do DirectAccess predefinida no modelo de certificado de autenticação da autoridade de registro.  
  
-   O período de validade deve ser definido como uma hora.  
  
    > [!NOTE]  
    > Em situações em que o servidor de CA é um computador com Windows Server 2003, o modelo deve ser configurado em um computador diferente. Isso se deve ao fato de que não é possível definir o **período de validade** em horas ao executar versões do Windows anteriores ao 2008/vista. Se o computador que você usa para configurar o modelo não tiver a função de serviço de certificação instalada, ou for um computador cliente, talvez seja necessário instalar o snap-in de modelos de certificado. Para obter mais informações sobre esse assunto, clique [aqui](https://technet.microsoft.com/library/cc732445.aspx).  
  
-   O período de renovação deve ser definido como 0.  
  
-   Adicional Os certificados e as solicitações não devem ser armazenados no banco de dados de autoridade de certificação.  
  
-   O parâmetro uso avançado de chave do certificado deve ser definido corretamente, da seguinte maneira:  
  
    -   Para o modelo de certificado de assinatura de registro do DirectAccess, use a chave 1.3.6.1.4.1.311.81.1.1.  
  
    -   Para o modelo de certificado de autenticação OTP, use a chave key 1.3.6.1.4.1.311.20.2.2.  
  
## <a name="bkmk_33RACert"></a>3,3 planejar o certificado de autoridade de registro  
Quando os clientes do DirectAccess solicitam um certificado de OTP, o servidor de acesso remoto recebe a solicitação do cliente. O servidor de acesso remoto assina todas as solicitações de certificado OTP de clientes usando o certificado de autoridade de registro. A AC emitirá certificados somente se a solicitação for assinada pelo certificado de autoridade de registro no servidor de acesso remoto. O certificado deve ser emitido por uma AC interna, o certificado não pode ser autoassinado. Ele não precisa ser emitido pela autoridade de certificação que emitiu os certificados OTP, mas a autoridade de certificação que emite os certificados OTP deve confiar na autoridade de certificação que emite o certificado de autenticação da autoridade de registro.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 4: planejar a OTP para o servidor de acesso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


