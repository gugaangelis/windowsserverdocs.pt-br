---
title: Acesso condicional para conectividade VPN usando o Azure AD
description: Nesta etapa opcional, você pode ajustar como os usuários de VPN autorizados acessam seus recursos usando o acesso condicional do Azure Active Directory (Azure AD).
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: da32df185cb0c0c2370e60119dd9c2fbd510bd08
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964258"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Etapa 7. Adicional Acesso condicional para conectividade VPN usando o Azure AD

- [**Anterior:** Etapa 6. Configurar conexões VPN Always On cliente do Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**Em seguida:** Etapa 7,1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Nesta etapa opcional, você pode ajustar como os usuários VPN acessam seus recursos usando o [acesso condicional do Azure Active Directory (AD do Azure)](/azure/active-directory/active-directory-conditional-access-azure-portal). Com o acesso condicional do Azure AD para conectividade de VPN (rede virtual privada), você pode ajudar a proteger as conexões VPN. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Pré-requisitos

Você está familiarizado com os seguintes tópicos:

- [Acesso condicional no Azure Active Directory](/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN e acesso condicional](/windows/access-protection/vpn/vpn-conditional-access)

Para configurar Azure Active Directory acesso condicional para conectividade VPN, você precisa ter os seguintes configurados:

- [Infraestrutura de servidor](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Servidor de acesso remoto para VPN Always On](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Servidor de Políticas de Rede](always-on-vpn/deploy/vpn-deploy-nps.md)
- [Configurações de DNS e firewall](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Conexões VPN Always On cliente do Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>[Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Nesta etapa, você pode adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui pontos de distribuição de CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado).

Um cliente EAP-TLS não pode se conectar, a menos que o servidor NPS conclua uma verificação de revogação da cadeia de certificados (incluindo o certificado raiz). Os certificados de nuvem emitidos para o usuário pelo Azure AD não têm uma CRL porque eles são certificados de curta duração com um tempo de vida de uma hora. O EAP no NPS precisa ser configurado para ignorar a ausência de uma CRL. Como o método de autenticação é EAP-TLS, esse valor de registro só é necessário em **EAP\13**. Se outros métodos de autenticação EAP forem usados, o valor do registro também deverá ser adicionado sob eles.

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-ad"></a>[Etapa 7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

Nesta etapa, você configura certificados raiz para autenticação de VPN com o Azure AD, que cria automaticamente um aplicativo de nuvem do servidor VPN no locatário.  

Para configurar o acesso condicional para conectividade VPN, você precisa:

1. Criar um certificado VPN no Portal do Azure.
2. Baixar o certificado VPN.
3. Implantar o certificado para o servidor VPN.

> [!IMPORTANT]
> Depois que um certificado VPN for criado na portal do Azure, o Azure AD começará a usá-lo imediatamente para emitir certificados de vida curta para o cliente VPN. É fundamental que o certificado VPN seja implantado imediatamente no servidor VPN para evitar problemas com a validação de credenciais do cliente VPN.

## <a name="step-73-configure-the-conditional-access-policy"></a>[Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)

Nesta etapa, você configura a política de acesso condicional para conectividade VPN.

Para configurar a política de acesso condicional, você precisa:

1. Crie uma política de acesso condicional que é atribuída a usuários VPN.
2. Defina o aplicativo de nuvem como **servidor VPN**.
3. Defina a concessão (controle de acesso) para **exigir a autenticação multifator**.  Você pode usar outros controles conforme necessário.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>[Etapa 7,4. Implantar certificados raiz de acesso condicional no AD local](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Nesta etapa, você implantará um certificado raiz confiável para autenticação de VPN em seu AD local.

Para implantar o certificado raiz confiável, você precisa:

1. Adicione o certificado baixado como uma *AC raiz confiável para autenticação de VPN*.
2. Importe o certificado raiz para o servidor VPN e o cliente VPN.
3. Verifique se os certificados estão presentes e mostram como confiáveis.

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>[Etapa 7.5. Criar perfis VPNv2 baseados em OMA-DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

Nesta etapa, você pode criar perfis de VPNv2 baseados em OMA-DM usando o Intune para implantar uma política de configuração de dispositivo VPN. Se você quiser usar o Configuration Manager ou o script do PowerShell para criar perfis do VPNv2, consulte [configurações do CSP do VPNv2](/windows/client-management/mdm/vpnv2-csp) para obter mais detalhes.

## <a name="next-steps"></a>Próximas etapas

[Etapa 7,1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md): nesta etapa, você deve adicionar **IgnoreNoRevocationCheck** e defini-la para permitir a autenticação de clientes quando o certificado não incluir pontos de distribuição da CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado).

## <a name="related-topics"></a>Tópicos relacionados

- [Configurar perfis de VPNv2](/windows/access-protection/vpn/vpn-conditional-access): o cliente VPN agora é capaz de se integrar à plataforma de acesso condicional baseada em nuvem para fornecer uma opção de conformidade de dispositivo para clientes remotos. Nesta etapa, você configura os perfis VPNv2 com ** \<DeviceCompliance> \<Enabled> true \</Enabled> **.

- [Aprimorando o acesso remoto no Windows 10 com um perfil VPN automático](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): saiba como a Microsoft implementa o acesso condicional para conectividade VPN. Os perfis de VPN contêm todas as informações que um dispositivo requer para se conectar à rede corporativa, incluindo os métodos de autenticação que têm suporte e o servidor VPN ao qual o dispositivo deve se conectar. Alterações na atualização de aniversário do Windows 10, incluindo acesso condicional e logon único, possibilitamos a criação de nosso perfil de conexão VPN AlwaysOn.

- [Acesso condicional no Azure Active Directory](/azure/active-directory/active-directory-conditional-access-azure-portal): a segurança é uma das principais preocupações para as organizações que usam a nuvem. Um aspecto importante da segurança em nuvem é a identidade e o acesso quando o assunto é gerenciar os recursos em nuvem. Em um mundo primeiro o dispositivo móvel e a nuvem, os usuários podem acessar os recursos da organização usando uma grande variedade de dispositivos e aplicativos de qualquer lugar. Por causa disso, concentrar-se apenas em quem pode acessar um recurso não é mais suficiente. Para dominar o equilíbrio entre segurança e produtividade, os profissionais de TI também precisam considerar como um recurso está sendo acessado em uma decisão de controle de acesso.

- [VPN e acesso condicional](/windows/access-protection/vpn/vpn-conditional-access): o cliente VPN agora é capaz de se integrar com a plataforma de acesso condicional baseada em nuvem para fornecer uma opção de conformidade de dispositivo para clientes remotos. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD).
