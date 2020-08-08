---
title: Configurar a política de acesso condicional
description: Após a criação de um certificado raiz, a ' conectividade VPN ' dispara a criação do aplicativo de nuvem ' servidor VPN ' no locatário do cliente.
ms.topic: article
ms.date: 05/25/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 7535de9f11a8daf38ad1aab2fe95a620f9682025
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946699"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>Etapa 7.3. Configure a política de acesso condicional

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 7,2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**Em seguida:** Etapa 7,4. Implantar certificados raiz de acesso condicional no AD local](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Nesta etapa, você configura a política de acesso condicional para conectividade VPN. Quando o primeiro certificado raiz é criado na folha ' conectividade VPN ', ele cria automaticamente um aplicativo de nuvem ' servidor VPN ' no locatário.

Crie uma política de acesso condicional atribuída ao grupo de usuários VPN e o escopo do **aplicativo de nuvem** para o **servidor VPN**:

- **Usuários**: usuários VPN
- **Aplicativo de nuvem**: servidor VPN
- **Grant (controle de acesso)**: ' exigir autenticação multifator '. Outros controles podem ser usados se desejado.

**Procedimento:** Esta etapa aborda a criação da política de acesso condicional mais básica.Se desejar, podem ser usadas condições e controles adicionais.


1. Na página **acesso condicional** , na barra de ferramentas na parte superior, selecione **Adicionar**.

    ![Selecionar Adicionar na página de acesso condicional](../../media/Always-On-Vpn/07.png)

2. Na página **novo** , na caixa **nome** , insira um nome para a política. Por exemplo, insira **política de VPN**.

    ![Adicionar nome de política na página de acesso condicional](../../media/Always-On-Vpn/08.png)

3. Na seção **atribuição** , selecione **usuários e grupos**.

    ![Selecionar Usuários e grupos](../../media/Always-On-Vpn/09.png)

4. Na página **Usuários e grupos**, execute as seguintes etapas:

    ![Selecionar usuário de teste](../../media/Always-On-Vpn/10.png)

    a. Selecione **Selecionar usuários e grupos**.

    b. Selecione **Selecionar**.

    c. Na página **selecionar** , selecione o grupo **usuários VPN** e selecione **selecionar**.

    d. Na página **usuários e grupos** , selecione **concluído**.

5. Na página **Novo**, realize as seguintes etapas:

    ![Selecionar aplicativos de nuvem](../../media/Always-On-Vpn/11.png)

    a. Na seção **atribuições** , selecione **aplicativos de nuvem**.

    b. Na página **aplicativos de nuvem** , selecione **selecionar aplicativos**.

    d. Selecione **Servidor VPN**.

6.  Na página **novo** , para abrir a página **concessão** , na seção **controles** , selecione **conceder**.

    ![Selecionar Conceder](../../media/Always-On-Vpn/13.png)

7.  Na página **Conceder**, execute as seguintes etapas:

    ![Selecionar Exigir autenticação multifator](../../media/Always-On-Vpn/14.png)

    a. Selecione **Exigir autenticação multifator**.

    b. Selecione **Selecionar**.

8.  Na página **novo** , em **habilitar política**, selecione **ativado**.

    ![Habilitar política](../../media/Always-On-Vpn/15.png)

9.  Na página **novo** , selecione **criar**.


## <a name="next-steps"></a>Próximas etapas
[Etapa 7,4. Implantar certificados raiz de acesso condicional para o AD local](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): nesta etapa, você implanta o certificado raiz de acesso condicional como certificado raiz confiável para autenticação de VPN para seu ad local.
