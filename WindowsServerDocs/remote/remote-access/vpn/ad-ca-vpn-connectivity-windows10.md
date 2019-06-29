---
title: Acesso condicional para conectividade VPN usando o Azure AD
description: Nesta etapa opcional, você pode ajustar como VPN os usuários autorizados acessam seus recursos usando o acesso condicional do Azure Active Directory (Azure AD).
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: f6383030f70dd7c0487edd534bcc0ad42010f409
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469308"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Etapa 7. (Opcional) Acesso condicional para conectividade VPN usando o Azure AD

- [**Anterior:** Etapa 6. Configurar as conexões da VPN Always On do cliente Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**Avançar:** Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Esta etapa opcional, você pode ajustar como usuários da VPN acessam seus recursos usando o [acesso condicional do Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Com acesso condicional do Azure AD para conectividade de rede virtual privada (VPN), você pode ajudar a proteger as conexões VPN. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Pré-requisitos

Você está familiarizado com os seguintes tópicos:

- [Acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Para configurar o acesso condicional do Azure Active Directory para conectividade VPN, você precisa ter os seguintes itens configurados:

- [Infraestrutura de servidor](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Servidor de acesso remoto VPN Always On](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Servidor de políticas de rede](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS e as configurações de Firewall](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Cliente do Windows 10 sempre em conexões VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação da CRL (lista de certificados revogados)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Nesta etapa, você pode adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui os pontos de distribuição da CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado).

Não é possível conectar a um cliente EAP-TLS, a menos que o servidor NPS conclui uma verificação de revogação da cadeia de certificados (incluindo o certificado raiz). Certificados de nuvem emitidos para o usuário pelo AD do Azure não tem uma CRL, porque eles são certificados de curta duração com um tempo de vida de uma hora. EAP no NPS deve ser configurado para ignorar a ausência de uma CRL. Como o método de autenticação EAP-TLS, esse valor de registro é necessário apenas sob **EAP\13**. Se outros métodos de autenticação EAP são usados, em seguida, o valor do registro deve ser adicionado abaixo deles também.

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[Etapa 7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

Nesta etapa, você pode configurar certificados raiz para autenticação de VPN com o Azure AD, que cria automaticamente um aplicativo de nuvem do servidor VPN no locatário.  

Para configurar o acesso condicional para conectividade VPN, você precisa:

1. Crie um certificado VPN no portal do Azure.
2. Baixe o certificado VPN.
3. Implante o certificado para o servidor VPN.

> [!IMPORTANT]
> Depois que um certificado de VPN é criado no portal do Azure, o Azure AD começará a usá-lo imediatamente para emitir certificados de vida útil curtos para o cliente VPN. É essencial que o certificado VPN ser implantada imediatamente para o servidor VPN para evitar quaisquer problemas com a validação de credencial do cliente VPN.

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)

Nesta etapa, você deve configurar a política de acesso condicional para conectividade VPN.

Para configurar a política de acesso condicional, você precisa:

1. Crie uma política de acesso condicional que é atribuída a usuários VPN.
2. Defina o aplicativo de nuvem **servidor VPN**.
3. Defina a concessão (controle de acesso) **exigir a autenticação multifator**.  Você pode usar outros controles conforme necessário.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[Etapa 7.4. Implantar certificados de raiz do acesso condicional para o local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Nesta etapa, você implanta um certificado raiz confiável para autenticação de VPN em suas instalações AD.

Para implantar o certificado raiz confiável, você precisa:

1. Adicionar o certificado baixado como um *AC raiz confiável para autenticação de VPN*.
2. Importe o certificado raiz para o servidor VPN e o cliente VPN.
3. Verifique se os certificados estão presentes e mostrarem como confiáveis.

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[Etapa 7.5. Criar perfis VPNv2 baseados em OMA-DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

Nesta etapa, você pode criar o OMA-DM com base em perfis de VPNv2 usando o Intune para implantar uma política de configuração do dispositivo VPN. Se você quiser usar o SCCM ou Script do PowerShell para criar perfis de VPNv2, consulte [configurações de CSP de VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obter mais detalhes.

## <a name="next-steps"></a>Próximas etapas

[Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação da lista de revogação de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): Nesta etapa, você deve adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui os pontos de distribuição da CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado).

## <a name="related-topics"></a>Tópicos relacionados

- [Configurar perfis de VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Agora o cliente VPN é capaz de integrar-se com a Plataforma de Acesso Condicional com base na nuvem para fornecer uma opção de conformidade do dispositivo para clientes remotos. Nesta etapa, você configura os perfis de VPNv2 com  **\<DeviceCompliance > \<habilitado > true\<habilitado >** .

- [Aprimorando o acesso remoto no Windows 10 com um perfil VPN automático](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Saiba como a Microsoft implementa o acesso condicional para conectividade VPN. Perfis de VPN contêm todas as informações de que um dispositivo exige para se conectar à rede corporativa, incluindo os métodos de autenticação que têm suporte e que o dispositivo deve se conectar ao servidor VPN. Alterações na atualização de aniversário do Windows 10, incluindo o acesso condicional e logon único, feita para que possamos criar nosso perfil de conexão de VPN Always-on. Criamos o perfil de conexão para o domínio e dispositivos gerenciados para Intune da Microsoft usando o console do System Center Configuration Manager.

- [Acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): A segurança é uma grande preocupação para organizações que usam a nuvem. Um aspecto importante da segurança na nuvem é a identidade e acesso quando se trata de gerenciar seus recursos de nuvem. Em um mundo de dispositivos móveis e nuvem em primeiro lugar, os usuários podem acessar recursos da sua organização usando uma variedade de dispositivos e aplicativos de qualquer lugar. Como resultado, concentrando-se apenas em quem pode acessar um recurso não é mais suficiente. Para dominar o equilíbrio entre segurança e produtividade, os profissionais de TI também precisam considerar como um recurso está sendo acessado em uma decisão de controle de acesso.

- [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Agora o cliente VPN é capaz de integrar-se com a Plataforma de Acesso Condicional com base na nuvem para fornecer uma opção de conformidade do dispositivo para clientes remotos. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD).
