---
title: Configurar a política de acesso condicional
description: Depois que um certificado raiz tiver sido criado, a conectividade VPN' ' dispara a criação do aplicativo de nuvem 'Servidor de VPN' no locatário do cliente.
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
ms.openlocfilehash: 466e76d01ca99a1e1ed72fa955ccd287ae63c5df
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749494"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>Etapa 7.3. Configurar a política de acesso condicional

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**Avançar:** Etapa 7.4. Implantar certificados de raiz do acesso condicional para o local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Nesta etapa, você deve configurar a política de acesso condicional para conectividade VPN. Quando o primeiro certificado raiz é criado na folha de 'Conectividade do VPN', ele cria automaticamente um aplicativo em nuvem 'Servidor de VPN' no locatário.

Criar uma política de acesso condicional que é atribuída a grupo de usuários VPN e o escopo de **aplicativo de nuvem** à **servidor VPN**:

- **Usuários**: Usuários VPN
- **Aplicativo de nuvem**: Servidor VPN
- **Grant (controle de acesso)** : 'Exigir a autenticação multifator'. Outros controles podem ser usados se desejado.

**Procedimento:** Esta etapa abrange a criação de política de acesso condicional mais básica.  Se desejar, condições e controles adicionais podem ser usados.


1. Sobre o **acesso condicional** página, na barra de ferramentas na parte superior, selecione **Add**.

    ![Selecionar Adicionar na página de acesso condicional](../../media/Always-On-Vpn/07.png)

2. No **New** página, o **nome** , digite um nome para sua política. Por exemplo, digite **política de VPN**.

    ![Adicionar nome de política na página de acesso condicional](../../media/Always-On-Vpn/08.png)

3. No **atribuição** seção, selecione **usuários e grupos**.

    ![Selecionar usuários e grupos](../../media/Always-On-Vpn/09.png)

4. Sobre o **usuários e grupos** , execute as seguintes etapas:

    ![Selecionar usuário de teste](../../media/Always-On-Vpn/10.png)

    a. Selecione **selecionar usuários e grupos**.

    b. Selecione **selecionar**.

    c. No **selecionar** , selecione o **usuários da VPN** agrupar e, em seguida, selecione **selecione**.

    d. Sobre o **usuários e grupos** página, selecione **feito**.

5. Sobre o **New** , execute as seguintes etapas:

    ![Selecionar aplicativos de nuvem](../../media/Always-On-Vpn/11.png)

    a. No **atribuições** seção, selecione **aplicativos de nuvem**.

    b. Sobre o **aplicativos de nuvem** página, selecione **selecionar aplicativos**.

    d. Selecione **servidor VPN**.

6.  No **New** página para abrir o **Grant** página, o **controles** seção, selecione **Grant**.

    ![Selecionar conceder](../../media/Always-On-Vpn/13.png)

7.  Sobre o **Grant** , execute as seguintes etapas:

    ![Selecione exigir a autenticação multifator](../../media/Always-On-Vpn/14.png)

    a. Selecione **exigir a autenticação multifator**.

    b. Selecione **selecionar**.

8.  No **New** página, em **habilitar política**, selecione **em**.

    ![Habilitar política](../../media/Always-On-Vpn/15.png)

9.  Sobre o **New** página, selecione **criar**.


## <a name="next-steps"></a>Próximas etapas
[Etapa 7.4. Implantar certificados de raiz do acesso condicional para o local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): Nesta etapa, você implanta o certificado de raiz do acesso condicional como certificado de raiz confiável para autenticação de VPN em suas instalações AD.
