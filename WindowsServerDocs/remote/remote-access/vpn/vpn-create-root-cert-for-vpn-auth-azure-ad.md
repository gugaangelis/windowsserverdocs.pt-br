---
title: Criar certificados raiz para autenticação de VPN com o Azure AD
description: O Azure AD usa o certificado VPN para assinar certificados emitidos para os clientes Windows 10 ao autenticar no Azure AD para conectividade VPN. O certificado marcado como primário é o emissor que o AD do Azure usa.
ms.topic: article
ms.date: 06/28/2019
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: dfcdea3b719cee685222fdf5919ce5e674ca1e64
ms.sourcegitcommit: 52a8d5d7e969eaa07fd3a45ed6d3cb5a5173b6d1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970633"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Etapa 7.2. Criar certificados raiz de acesso condicional para autenticação VPN com o Azure AD

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 7,1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**Em seguida:** Etapa 7,3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)

Nesta etapa, você configura certificados raiz de acesso condicional para autenticação VPN com o Azure AD, que cria automaticamente um aplicativo de nuvem chamado servidor VPN no locatário. Para configurar o acesso condicional para conectividade VPN, você precisa:

1. Criar um certificado VPN no Portal do Azure.
2. Baixar o certificado VPN.
3. Implante o certificado nos servidores VPN e NPS.

> [!IMPORTANT]
> Depois que um certificado VPN for criado na portal do Azure, o Azure AD começará a usá-lo imediatamente para emitir certificados de vida curta para o cliente VPN. É fundamental que o certificado VPN seja implantado imediatamente no servidor VPN para evitar problemas com a validação de credenciais do cliente VPN.

Quando um usuário tenta uma conexão VPN, o cliente VPN faz uma chamada para o WAM (Web Account Manager) no cliente Windows 10. O WAM faz uma chamada para o aplicativo de nuvem do servidor VPN. Quando as condições e os controles na política de acesso condicional são satisfeitos, o Azure AD emite um token na forma de um certificado de curta duração (1 hora) para o WAM. O WAM coloca o certificado no repositório de certificados do usuário e passa o controle para o cliente VPN. 

Em seguida, o cliente VPN envia o certificado emitido pelo Azure AD para a VPN para validação de credenciais. 

> [!NOTE]
> O Azure AD usa o certificado criado mais recentemente na folha de conectividade de VPN como o emissor.

**Procedure**

1. Entre no [Portal do Azure](https://portal.azure.com) como administrador global.
2. No menu à esquerda, clique em **Azure Active Directory**.
3. Na página **Azure Active Directory** , na seção **gerenciar** , clique em **segurança**.
4. Na página **segurança** , na seção **proteger** , clique em **acesso condicional**.
5. No **acesso condicional | Política** , na seção **gerenciar** , clique em **conectividade VPN**.
5. Na página **Conectividade VPN**, clique em **Novo certificado**.
6. Na página **novo** , execute as seguintes etapas: a. Para **selecionar duração**, selecione 1, 2 ou 3 anos.
    b. Selecione **Criar**.

## <a name="next-steps"></a>Próximas etapas

[Etapa 7,3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md): nesta etapa, você configura a política de acesso condicional para conectividade VPN.
