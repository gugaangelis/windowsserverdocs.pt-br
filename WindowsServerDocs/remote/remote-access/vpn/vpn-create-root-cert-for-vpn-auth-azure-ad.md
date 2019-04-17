---
title: Criar certificados raiz para autenticação de VPN com o Azure AD
description: Azure AD usa o certificado VPN para assinar certificados emitidos para clientes do Windows 10 ao autenticar no Azure AD para conectividade VPN. O certificado marcado como principal é o emissor que usa o Azure AD.
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
ms.openlocfilehash: 14ef17ab403cc4e7c9891f4ede48e41c25e8522d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066961"
---
# Etapa 7.2. Criar certificados raiz para autenticação de VPN de acesso condicional com o Azure AD

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** etapa 7.1. Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)<br>
& #187; [ **Próximo:** etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)

Nesta etapa, você pode configurar certificados raiz de acesso condicional para autenticação de VPN com o Azure AD, que automaticamente cria um aplicativo de nuvem chamado servidor VPN no locatário. Para configurar o acesso condicional para conectividade VPN, você precisa:

1. Crie um certificado VPN no portal do Azure (você pode criar mais de um certificado).
2. Baixe o certificado VPN.
2. Implante o certificado para seus servidores NPS e VPN.

Quando um usuário tenta uma conexão VPN, o cliente VPN faz uma chamada no Gerenciador de conta da Web (WAM) no cliente do Windows 10. WAM faz uma chamada para o aplicativo de nuvem do servidor VPN. Quando os controles na política de acesso condicional e condições forem atendidos, o Azure AD emite um token na forma de um certificado de curta duração (1 hora) para o WAM. O WAM coloca o certificado no repositório de certificados do usuário e passa o controle para o cliente VPN.  

O cliente VPN, em seguida, envia os problemas de certificado pelo Azure AD à VPN para validação de credenciais.Azure AD usa o certificado que está marcado como**principal**na folha conectividade VPN como o emissor. 

No portal do Azure, você deve criar dois certificados para gerenciar a transição quando um certificado está prestes a expirar. Quando você cria um certificado, você escolhe se ele é o certificado principal, que é usado durante a autenticação para assinar o certificado para a conexão.

**Procedimento:**

1. Entre no seu [portal do Azure](https://portal.azure.com) como um administrador global.

2. No menu à esquerda, clique em **Azure Active Directory**. 

    ![Selecione Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. Na página do **Azure Active Directory** , na seção **Gerenciar** , clique em **acesso condicional**.

    ![Selecione o acesso condicional](../../media/Always-On-Vpn/02.png)

4. Na página **acesso condicional** , na seção **Gerenciar** , clique em **conectividade VPN (visualização)**.

    ![Selecione a conectividade VPN](../../media/Always-On-Vpn/03.png)

5. Na página **conectividade VPN** , clique em **novo certificado**.

    ![Selecione o novo certificado](../../media/Always-On-Vpn/04.png)

6. Na página **nova** , execute as seguintes etapas:

    ![Selecione duração e principal](../../media/Always-On-Vpn/05.png)

    a. Para **Selecionar a duração**, selecione 1 ou 2 anos. Você pode adicionar até duas certificados para gerenciar transições quando o certificado está prestes a expirar. Você pode escolher qual deles é o principal (aquele usado durante a autenticação para assinar o certificado para conectividade).

    b. Para **principal**, selecione **Sim**.

    c. Clique em **Criar**.

## Próximas etapas
Etapa [7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md): nesta etapa, você configura a política de acesso condicional para conectividade VPN. 

---