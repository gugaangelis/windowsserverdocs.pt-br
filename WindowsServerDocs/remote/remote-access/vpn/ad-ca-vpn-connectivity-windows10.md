---
title: Acesso condicional para conectividade VPN usando o Azure AD
description: Nesta etapa opcional, você pode ajustar acesso de usuários autorizado como VPN seus recursos usando o acesso condicional do Azure Active Directory (Azure AD).
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 07/13/2018
ms.reviewer: deverette
ms.openlocfilehash: c9104a5d9ae3069e753b8b771270502c4264db96
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066963"
---
# Etapa 7. (Opcional) Acesso condicional para conectividade VPN usando o Azure AD

& #171;  [ **Anterior:** etapa 6. Configurar o cliente do Windows 10 sempre em conexões VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)<br>
& #187; [ **Próximo:** etapa 7.1. Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Nesta etapa opcional, você pode ajustar como os usuários da VPN acessam seus recursos usando o [acesso condicional do Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Com acesso condicional do Azure AD para conectividade de rede virtual privada (VPN), você pode ajudar a proteger as conexões VPN. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD). 

## Pré-requisitos

Você está familiarizado com os seguintes tópicos:
- [Acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Para configurar o acesso condicional do Azure Active Directory para conectividade VPN, você precisa ter os seguintes itens configurados:
- [Infraestrutura de servidor](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Servidor de acesso remoto para sempre ativado VPN](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Servidor de Políticas de Rede](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS e as configurações do Firewall](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Cliente do Windows 10 sempre em conexões VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## [Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Nesta etapa, você pode adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui pontos de distribuição de CRL. Por padrão, IgnoreNoRevocationCheck é definida como 0 (desabilitado).

Um cliente de EAP-TLS não pode se conectar, a menos que o servidor NPS conclui a verificação de revogação de cadeia de certificados (incluindo o certificado raiz). Certificados de nuvem emitidos para o usuário pelo Azure AD não têm uma CRL porque eles são certificados de curta duração com uma vida útil de uma hora. EAP no NPS precisa ser configurado para ignorar a ausência de uma lista de certificados Revogados. Como o método de autenticação EAP-TLS, esse valor do registro é necessário somente em **EAP\13**. Se outros métodos de autenticação EAP forem usados, em seguida, o valor do registro deve ser adicionado em aqueles também. 




## [Etapa 7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

Nesta etapa, você pode configurar certificados raiz para autenticação de VPN com o Azure AD, que automaticamente cria um aplicativo de nuvem do servidor VPN no locatário.  

Para configurar o acesso condicional para conectividade VPN, você precisa:
1. Crie um certificado VPN no portal do Azure (você pode criar mais de um certificado).
2. Baixe o certificado VPN.
3. Implante o certificado para o servidor VPN.

## [Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)

Nesta etapa, você configura a política de acesso condicional para conectividade VPN. 

Para configurar a política de acesso condicional, você precisa:
1. Crie uma política de acesso condicional que é atribuída para usuários de VPN.
2. Defina o aplicativo de nuvem como **servidor VPN**.
3. Defina a concessão (controle de acesso) para **exigir autenticação multifator**.  Você pode usar outros controles conforme necessário.

## [Etapa 7.4. Implantar certificados de raiz de acesso condicional para locais AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Nesta etapa, você implanta um certificado raiz confiável para autenticação de VPN em seu local AD.

Para implantar o certificado raiz confiável, você precisa:
1. Adicione o certificado baixado como uma *raiz confiável autoridade de certificação de autenticação VPN*.
2. Importe o certificado raiz para o servidor VPN e o cliente VPN.
3. Verifique se os certificados estão presentes e mostram como confiáveis.


## [Etapa 7.5. Criar perfis de VPNv2 baseados em OMA-DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

Nesta etapa, você pode criar OMA DM com base em perfis de VPNv2 usando o Intune para implantar uma política de configuração de dispositivo da VPN. Se você quiser usar o SCCM ou Script do PowerShell para criar perfis de VPNv2, consulte [configurações do CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obter mais detalhes. 


## Próximas etapas
Etapa [7.1. Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): nesta etapa, você deve adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui pontos de distribuição de CRL. Por padrão, IgnoreNoRevocationCheck é definida como 0 (desabilitado).

---

## Tópicos relacionados
- [Configurar perfis de VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): agora o cliente a VPN é capaz de integrar com a plataforma baseada em nuvem de acesso a condicional para oferecer uma opção de conformidade do dispositivo para clientes remotos. Nesta etapa, você configura os perfis de VPNv2 com **\ < DeviceCompliance > \ < habilitado > true\ < / habilitado >**. 
 
- [Aprimorando acesso remoto no Windows 10 com um perfil VPN automática](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Saiba como o Microsoft implementa o acesso condicional para conectividade VPN. Perfis de VPN contenham todas as informações do que dispositivo requer para se conectar à rede corporativa, incluindo os métodos de autenticação que têm suporte e que o dispositivo deve se conectar ao servidor VPN. Alterações na atualização de aniversário do Windows 10, incluindo o acesso condicional e logon único, feitas nos permite criar nosso perfil de conexão VPN Always-On. Criamos o perfil de conexão para ingressado no domínio e Microsoft Intune – gerenciado dispositivos usando o console do System Center Configuration Manager. 

- [Condicional acessar no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): segurança é uma grande preocupação para organizações que usam a nuvem. Um aspecto importante da segurança de nuvem é a identidade e acesso quando se trata de gerenciar seus recursos de nuvem. Em um mundo prioriza, prioriza a nuvem, os usuários podem acessar os recursos da sua organização usando uma variedade de dispositivos e aplicativos de qualquer lugar. Como resultado, concentrando-se apenas em quem pode acessar um recurso não é suficiente mais. Para o equilíbrio entre a segurança e a produtividade do mestre, os profissionais de TI também precisam fatorar como um recurso está sendo acessado em uma decisão de controle de acesso.

- [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): agora o cliente a VPN é capaz de integrar com a plataforma baseada em nuvem de acesso a condicional para oferecer uma opção de conformidade do dispositivo para clientes remotos. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD). 

---