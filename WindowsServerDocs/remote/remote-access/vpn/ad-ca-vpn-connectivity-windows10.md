---
title: Acesso condicional para conectividade VPN usando o Azure AD
description: Nesta etapa opcional, você pode ajustar como os usuários de VPN autorizados acessam seus recursos usando o acesso condicional do Azure Active Directory (Azure AD).
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: be50c8eaf789b6f0737cbe07cf10d041d25e74f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388204"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Etapa 7. Adicional Acesso condicional para conectividade VPN usando o Azure AD

- [**Anterior** Etapa 6. Configurar as conexões da VPN Always On do cliente Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**Última** Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Nesta etapa opcional, você pode ajustar como os usuários VPN acessam seus recursos usando o [acesso condicional do Azure Active Directory (AD do Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Com o acesso condicional do Azure AD para conectividade de VPN (rede virtual privada), você pode ajudar a proteger as conexões VPN. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Pré-requisitos

Você está familiarizado com os seguintes tópicos:

- [Acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Para configurar Azure Active Directory acesso condicional para conectividade VPN, você precisa ter os seguintes configurados:

- [Infraestrutura de servidor](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Servidor de acesso remoto para VPN Always On](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Servidor de políticas de rede](always-on-vpn/deploy/vpn-deploy-nps.md)
- [Configurações de DNS e firewall](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Conexões VPN Always On cliente do Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Nesta etapa, você pode adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui pontos de distribuição de CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado).

Um cliente EAP-TLS não pode se conectar, a menos que o servidor NPS conclua uma verificação de revogação da cadeia de certificados (incluindo o certificado raiz). Os certificados de nuvem emitidos para o usuário pelo Azure AD não têm uma CRL porque eles são certificados de curta duração com um tempo de vida de uma hora. O EAP no NPS precisa ser configurado para ignorar a ausência de uma CRL. Como o método de autenticação é EAP-TLS, esse valor de registro só é necessário em **EAP\13**. Se outros métodos de autenticação EAP forem usados, o valor do registro também deverá ser adicionado sob eles.

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[Etapa 7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

Nesta etapa, você configura certificados raiz para autenticação de VPN com o Azure AD, que cria automaticamente um aplicativo de nuvem do servidor VPN no locatário.  

Para configurar o acesso condicional para conectividade VPN, você precisa:

1. Crie um certificado VPN no portal do Azure.
2. Baixe o certificado VPN.
3. Implante o certificado no servidor VPN.

> [!IMPORTANT]
> Depois que um certificado VPN for criado na portal do Azure, o Azure AD começará a usá-lo imediatamente para emitir certificados de vida curta para o cliente VPN. É fundamental que o certificado VPN seja implantado imediatamente no servidor VPN para evitar problemas com a validação de credenciais do cliente VPN.

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)

Nesta etapa, você configura a política de acesso condicional para conectividade VPN.

Para configurar a política de acesso condicional, você precisa:

1. Crie uma política de acesso condicional que é atribuída a usuários VPN.
2. Defina o aplicativo de nuvem como **servidor VPN**.
3. Defina a concessão (controle de acesso) para **exigir a autenticação multifator**.  Você pode usar outros controles conforme necessário.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[Etapa 7.4. Implantar certificados raiz de acesso condicional no AD local @ no__t-0

Nesta etapa, você implantará um certificado raiz confiável para autenticação de VPN em seu AD local.

Para implantar o certificado raiz confiável, você precisa:

1. Adicione o certificado baixado como uma *AC raiz confiável para autenticação de VPN*.
2. Importe o certificado raiz para o servidor VPN e o cliente VPN.
3. Verifique se os certificados estão presentes e mostram como confiáveis.

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[Etapa 7.5. Criar perfis VPNv2 baseados em OMA-DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

Nesta etapa, você pode criar perfis de VPNv2 baseados em OMA-DM usando o Intune para implantar uma política de configuração de dispositivo VPN. Se você quiser usar o script do SCCM ou do PowerShell para criar perfis do VPNv2, consulte [configurações do CSP do VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obter mais detalhes.

## <a name="next-steps"></a>Próximas etapas

[Etapa 7.1. Configure o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados) @ no__t-0: Nesta etapa, você deve adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui pontos de distribuição de CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado).

## <a name="related-topics"></a>Tópicos relacionados

- [Configurar perfis de VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Agora o cliente VPN é capaz de integrar-se com a Plataforma de Acesso Condicional com base na nuvem para fornecer uma opção de conformidade do dispositivo para clientes remotos. Nesta etapa, você configurará os perfis VPNv2 com **\<DeviceCompliance > \<Enabled > no__t-3/Enabled >** .

- [Aprimorando o acesso remoto no Windows 10 com um perfil VPN automático](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Saiba como a Microsoft implementa o acesso condicional para conectividade VPN. Os perfis de VPN contêm todas as informações que um dispositivo requer para se conectar à rede corporativa, incluindo os métodos de autenticação que têm suporte e o servidor VPN ao qual o dispositivo deve se conectar. Alterações na atualização de aniversário do Windows 10, incluindo acesso condicional e logon único, possibilitamos a criação de nosso perfil de conexão VPN AlwaysOn. Criamos o perfil de conexão para dispositivos gerenciados pelo domínio e Microsoft Intune – usando System Center Configuration Manager Console.

- [Acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): A segurança é uma das principais preocupações para as organizações que usam a nuvem. Um aspecto fundamental da segurança na nuvem é a identidade e o acesso quando se trata de gerenciar seus recursos de nuvem. Em um mundo em primeiro lugar e em nuvem, os usuários podem acessar os recursos da sua organização usando uma variedade de dispositivos e aplicativos de qualquer lugar. Como resultado disso, apenas se concentrando em quem pode acessar um recurso não é mais suficiente. Para dominar o equilíbrio entre segurança e produtividade, os profissionais de ti também precisam fatorar como um recurso está sendo acessado em uma decisão de controle de acesso.

- [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Agora o cliente VPN é capaz de integrar-se com a Plataforma de Acesso Condicional com base na nuvem para fornecer uma opção de conformidade do dispositivo para clientes remotos. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD).
