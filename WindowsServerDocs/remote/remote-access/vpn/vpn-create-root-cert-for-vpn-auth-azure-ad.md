---
title: Criar certificados raiz para autenticação de VPN com o Azure AD
description: Azure AD usa o certificado de VPN para assinar certificados emitidos para clientes do Windows 10 ao autenticar no Azure AD para conectividade VPN. O certificado marcado como primário é o emissor que usa o Azure AD.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: dc24f0275e8639ffd972ae24550d0ada38eff4f1
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749634"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Etapa 7.2. Criar certificados raiz para autenticação de VPN de acesso condicional com o Azure AD

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**Avançar:** Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)

Nesta etapa, você pode configurar certificados de raiz do acesso condicional para autenticação de VPN com o Azure AD, que cria automaticamente um aplicativo de nuvem chamado servidor VPN no locatário. Para configurar o acesso condicional para conectividade VPN, você precisa:

1. Crie um certificado VPN no portal do Azure (você pode criar mais de um certificado).
2. Baixe o certificado VPN.
3. Implante o certificado para seus servidores VPN e o NPS.

Quando um usuário tenta uma conexão VPN, o cliente VPN faz uma chamada no Gerenciador de conta da Web (WAM) no cliente Windows 10. WAM faz uma chamada para o aplicativo de nuvem do servidor VPN. Quando os controles na política de acesso condicional e as condições forem atendidos, o Azure AD emite um token na forma de um certificado (1 hora) e de curta duração para a WAM. O WAM coloca o certificado no repositório de certificados do usuário e passa o controle para o cliente VPN.  

O cliente VPN, em seguida, envia os problemas de certificado pelo Azure AD para a VPN para validação de credenciais.  O Azure AD usa o certificado que está marcado como **primário** na folha de conectividade de VPN como o emissor. 

No portal do Azure, você deve criar dois certificados para gerenciar as transições quando um certificado está prestes a expirar. Quando você cria um certificado, você escolha se é o certificado primário, que é usado durante a autenticação para assinar o certificado para a conexão.

**Procedimento:**

1. Entrar no seu [portal do Azure](https://portal.azure.com) como um administrador global.

2. No menu à esquerda, clique em **Azure Active Directory**. 

    ![Selecione Active Directory do Azure](../../media/Always-On-Vpn/01.png)

3. No **Azure Active Directory** página, o **gerenciar** seção, clique em **acesso condicional**.

    ![Selecionar o acesso condicional](../../media/Always-On-Vpn/02.png)

4. No **acesso condicional** página, o **gerenciar** seção, clique em **conectividade VPN (versão prévia)** .

    ![Selecionar a conectividade VPN](../../media/Always-On-Vpn/03.png)

5. Sobre o **conectividade VPN** , clique em **novo certificado**.

    ![Selecione o novo certificado](../../media/Always-On-Vpn/04.png)

6. Sobre o **New** , execute as seguintes etapas:

    ![Selecionar duração e primário](../../media/Always-On-Vpn/05.png)

    a. Para **selecionar duração**, selecione 1 ou 2 anos. Você pode adicionar até dois certificados para gerenciar as transições quando o certificado está prestes a expirar. Você pode escolher qual deles é o primário (aquele usado durante a autenticação para assinar o certificado para conectividade).

    b. Para **primário**, selecione **Sim**.

    c. Selecione **Criar**.

## <a name="next-steps"></a>Próximas etapas

[Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md): Nesta etapa, você deve configurar a política de acesso condicional para conectividade VPN. 
