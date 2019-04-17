---
title: Configurar a política de acesso condicional
description: Após ter sido criado um certificado raiz, a conectividade VPN' ' aciona a criação do aplicativo de nuvem 'Servidor VPN' no locatário do cliente.
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
ms.openlocfilehash: 8c00855c50de79efa1b48c7b8762e1b679db4a87
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066962"
---
# Etapa 7.3. Configurar a política de acesso condicional

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** etapa 7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)<br>
& #187; [ **Próximo:** etapa 7.4. Implantar certificados de raiz de acesso condicional para locais AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Nesta etapa, você configura a política de acesso condicional para conectividade VPN. Quando o primeiro certificado raiz é criado na folha 'Conectividade VPN', ele automaticamente cria um aplicativo em nuvem 'Servidor VPN' no locatário. 

Crie uma política de acesso condicional que é atribuída ao grupo de usuários VPN e o escopo do **aplicativo de nuvem** para **Servidor VPN**: 

- **Os usuários**: os usuários da VPN
- **Aplicativo de nuvem**: servidor VPN
- **Conceder (controle de acesso)**: 'Exigir autenticação multifator'. Outros controles podem ser usados, se desejado.

**Procedimento:** Esta etapa aborda a criação da política de acesso condicional mais básica.Se desejado, condições e controles adicionais podem ser usados.


1. Na página **Acesso condicional** , na barra de ferramentas na parte superior, clique em **Adicionar**.

    ![Selecione Adicionar na página de acesso condicional](../../media/Always-On-Vpn/07.png)

2. Na página **nova** , na caixa **nome** , digite um nome para sua política. Por exemplo, digite a **política de VPN**.

    ![Adicione o nome para a política na página de acesso condicional](../../media/Always-On-Vpn/08.png)

3. Na seção de **atribuição** , clique em **usuários e grupos**.

    ![Selecione os usuários e grupos](../../media/Always-On-Vpn/09.png)

4. Na página de **usuários e grupos** , execute as seguintes etapas:

    ![Usuário de teste selecione](../../media/Always-On-Vpn/10.png)

    a. Clique em **Selecionar usuários e grupos**.

    b. Clique em **Selecionar**.

    c. Na página **Selecionar** , selecione o grupo de **usuários de VPN** e, em seguida, clique em **Selecionar**.

    d. Na página de **usuários e grupos** , clique em **concluído**.

5. Na página **nova** , execute as seguintes etapas:

    ![Selecione os aplicativos de nuvem](../../media/Always-On-Vpn/11.png)

    a. Na seção **atribuições** , clique em **aplicativos de nuvem**.

    b. Na página **aplicativos de nuvem** , clique em **Selecionar aplicativos**.

    d. Selecione o **servidor VPN**.

13. Na página **nova** , para abrir a página de **concessão** , na seção **controles** , clique em **Grant**.

    ![Concessão de seleção](../../media/Always-On-Vpn/13.png)

14. Na página **Grant** , execute as seguintes etapas:

    ![Selecione exigir autenticação multifator](../../media/Always-On-Vpn/14.png)

    a. Selecione **exigir autenticação multifator**.

    b. Clique em **Selecionar**.

15. Na página **nova** , em **Habilitar política**, clique **em**.

    ![Habilitar política](../../media/Always-On-Vpn/15.png)

16. Na página **nova** , clique em **criar**.


## Próximas etapas
Etapa [7.4. Implantar certificados de raiz de acesso condicional para locais AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): nesta etapa, você implantar o certificado raiz de acesso condicional como certificado raiz confiável para autenticação de VPN para seus locais AD.

---