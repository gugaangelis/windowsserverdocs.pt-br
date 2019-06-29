---
title: Criar certificados raiz para autenticação de VPN com o Azure AD
description: Azure AD usa o certificado de VPN para assinar certificados emitidos para clientes do Windows 10 ao autenticar no Azure AD para conectividade VPN. O certificado marcado como primário é o emissor que usa o Azure AD.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 40403f6be65c84cc1e2506a222a009400fcdaacc
ms.sourcegitcommit: 34232723f15c7b4d6a32ca38b7348a417ba30ae0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67464955"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Etapa 7.2. Criar certificados raiz para autenticação de VPN de acesso condicional com o Azure AD

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**Avançar:** Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)

Nesta etapa, você pode configurar certificados de raiz do acesso condicional para autenticação de VPN com o Azure AD, que cria automaticamente um aplicativo de nuvem chamado servidor VPN no locatário. Para configurar o acesso condicional para conectividade VPN, você precisa:

1. Crie um certificado VPN no portal do Azure.
2. Baixe o certificado VPN.
3. Implante o certificado para seus servidores VPN e o NPS.

> [!IMPORTANT]
> Depois que um certificado de VPN é criado no portal do Azure, o Azure AD começará a usá-lo imediatamente para emitir certificados de vida útil curtos para o cliente VPN. É essencial que o certificado VPN ser implantada imediatamente para o servidor VPN para evitar quaisquer problemas com a validação de credencial do cliente VPN.

Quando um usuário tenta uma conexão VPN, o cliente VPN faz uma chamada no Gerenciador de conta da Web (WAM) no cliente Windows 10. WAM faz uma chamada para o aplicativo de nuvem do servidor VPN. Quando os controles na política de acesso condicional e as condições forem atendidos, o Azure AD emite um token na forma de um certificado (1 hora) e de curta duração para a WAM. O WAM coloca o certificado no repositório de certificados do usuário e passa o controle para o cliente VPN.  

O cliente VPN, em seguida, envia os problemas de certificado pelo Azure AD para a VPN para validação de credenciais.  

> [!NOTE]
> Azure AD usa o certificado criado mais recentemente na folha de conectividade de VPN como o emissor.

**Procedimento:**

1. Entrar no seu [portal do Azure](https://portal.azure.com) como um administrador global.
2. No menu à esquerda, clique em **Azure Active Directory**.
3. No **Azure Active Directory** página, o **gerenciar** seção, clique em **acesso condicional**.
4. No **acesso condicional** página, o **gerenciar** seção, clique em **conectividade VPN (versão prévia)** .
5. Sobre o **conectividade VPN** , clique em **novo certificado**.
6. Sobre o **New** , execute as etapas a seguir: um. Para **selecionar duração**, selecione 1, 2 ou 3 anos.
   b. Selecione **Criar**.

## <a name="next-steps"></a>Próximas etapas

[Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md): Nesta etapa, você deve configurar a política de acesso condicional para conectividade VPN.
